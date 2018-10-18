# Deploy Recipes<a name="create-custom-deploy"></a>

Deploy recipes are assigned to the layer's Deploy [lifecycle](workingcookbook-events.md) event\. It typically occurs on all of the stack's instances whenever you deploy an app, although you can optionally restrict the event to only specified instances\. AWS OpsWorks Stacks also runs the Deploy recipes on new instances, after the Setup recipes complete\. The primary purpose of Deploy recipes is to deploy code and related files from a repository to the application server layer's instances\. However, you often run Deploy recipes on other layers as well\. This allows those layers' instances, for example, to update their configuration to accommodate the newly deployed app\. When you implement a Deploy recipe, keep in mind a Deploy event does not necessarily mean that apps are being deployed to the instance\. It could simply be a notification that apps are being deployed to other instances in the stack, to allow the instance to make any necessary updates\. The recipe must be able to respond appropriately, which might mean doing nothing\.

AWS OpsWorks Stacks automatically deploys apps of the standard app types to the corresponding built\-in application server layers\. To deploy apps to a custom layer, you must implement custom Deploy recipes that download the app's files from a repository to the appropriate location on the instance\. However, you can often limit the amount of code you must write by using the built\-in [deploy cookbook](https://github.com/aws/opsworks-cookbooks/tree/release-chef-11.4/deploy) to handle some aspects of deployment\. For example, if you store your files in one of the supported repositories, the built\-in cookbook can handle the details of downloading the files from the repository to the layer's instances\. 

The `tomcat::deploy` recipe is intended to be assigned to the Deploy lifecycle event\.

```
include_recipe 'deploy'

node[:deploy].each do |application, deploy|
  opsworks_deploy_dir do
    user deploy[:user]
    group deploy[:group]
    path deploy[:deploy_to]
  end

  opsworks_deploy do
    deploy_data deploy
    app application
  end
...
```

The `tomcat::deploy` recipe uses the built\-in deploy cookbook for aspects of deployment that aren't application specific\. The `deploy` recipe \(which is shorthand for the built\-in `deploy::default` recipe\) is a built\-in recipe that handles the details of setting up the users, groups, and so on, based on data from the `deploy` attributes\.

The recipe uses two built\-in Chef definitions, `opsworks_deploy_dir` and `opworks_deploy` to install the application\. 

The `opsworks_deploy_dir` definition sets up the directory structure, based on data from the app's deployment JSON\. Definitions are basically a convenient way to package resource definitions, and are located in a cookbook's `definitions` directory\. Recipes can use definitions much like resources, but the definition itself does not have an associated provider, just the resources that are included in the definition\. You can define variables in the recipe, which are passed to the underlying resource definitions\. The `tomcat::deploy` recipe sets `user`, `group`, and `path` variables based on data from the deployment JSON\. They are passed to the definition's [directory resource](https://docs.chef.io/chef/resources.html#directory), which manages the directories\. 

**Note**  
Your deployed app's user and group are determined by the `[:opsworks][:deploy_user][:user]` and `[:opsworks][:deploy_user][:group]` attributes, which are defined in the [built\-in deploy cookbook's `deploy.rb` attributes file](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.4/deploy/attributes/deploy.rb)\. The default value of `[:opsworks][:deploy_user][:user]` is `deploy`\. The default value of `[:opsworks][:deploy_user][:group]` depends on the instance's operating system:  
For Ubuntu instances, the default group is `www-data`\.
For Amazon Linux instances that are members of a Rails App Server layer that uses Nginx and Unicorn, the default group is `nginx`\.
For all other Amazon Linux instances, the default group is `apache`\.
You can change either setting by using custom JSON or a custom attributes file to override the appropriate attribute\. For more information, see [Overriding Attributes](workingcookbook-attributes.md)\.

The other definition, `opsworks_deploy`, handles the details of checking out the app's code and related files from the repository and deploying them to the instance, based on data from the `deploy` attributes\. You can use this definition for any app type; deployment details such as the directory names are specified in the console or through the API and put in the `deploy` attributes\. However, `opsworks_deploy` works only for the four [supported repository types](workingcookbook-installingcustom-repo.md): Git, Subversion, S3, and HTTP\. You must implement this code yourself if you want to use a different repository type\.

You install an app's files in the Tomcat `webapps` directory\. A typical practice is to copy the files directly to `webapps`\. However, AWS OpsWorks Stacks deployment is designed to retain up to five versions of an app on an instance, so you can roll back to an earlier version if necessary\. AWS OpsWorks Stacks therefore does the following:

1. Deploys apps to a distinct directory whose name contains a time stamp, such as `/srv/www/my_1st_jsp/releases/20130731141527`\.

1. Creates a symlink named `current`, such as `/srv/www/my_1st_jsp/current`, to this unique directory\.

1. If does not already exist, creates a symlink from the `webapps` directory to the `current` symlink created in Step 2\.

If you need to roll back to an earlier version, modify the `current` symlink to point to a distinct directory containing the appropriate timestamp, for example, by changing the link target of `/srv/www/my_1st_jsp/current`\.

The middle section of `tomcat::deploy` sets up the symlink\. 

```
  ...
  current_dir = ::File.join(deploy[:deploy_to], 'current')
  webapp_dir = ::File.join(node['tomcat']['webapps_base_dir'], deploy[:document_root].blank? ? application : deploy[:document_root])

  # opsworks_deploy creates some stub dirs, which are not needed for typical webapps
  ruby_block "remove unnecessary directory entries in #{current_dir}" do
    block do
      node['tomcat']['webapps_dir_entries_to_delete'].each do |dir_entry|
        ::FileUtils.rm_rf(::File.join(current_dir, dir_entry), :secure => true)
      end
    end
  end

  link webapp_dir do
    to current_dir
    action :create
  end
  ...
```

The recipe first creates two variables, `current_dir` and `webapp_dir` to represent the `current` and `webapp` directories, respectively\. It then uses a `link` resource to link `webapp_dir` to `current_dir`\. The AWS OpsWorks Stacks `deploy::default` recipe creates some stub directories that aren't required for this example, so the middle part of the excerpt removes them\.

The final part of `tomcat::deploy` restarts the Tomcat service, if necessary\.

```
  ...
  include_recipe 'tomcat::service'

  execute 'trigger tomcat service restart' do
    command '/bin/true'
    not_if { node['tomcat']['auto_deploy'].to_s == 'true' }
    notifies :restart, resources(:service => 'tomcat')
  end
end

include_recipe 'tomcat::context'
```

The recipe first runs `tomcat::service`, to ensure that the service is defined for this Chef run\. It then uses an [execute resource](https://docs.chef.io/chef/resources.html#execute) to notify the service to restart, but only if `['tomcat']['auto_deploy']` is set to `'true'`\. Otherwise, Tomcat listens for changes in its `webapps` directory, which makes an explicit Tomcat service restart unnecessary\. 

**Note**  
The `execute` resource doesn't actually execute anything substantive; `/bin/true` is a dummy shell script that simply returns a success code\. It is used here simply as a convenient way to generate a restart notification\. As mentioned earlier, using notifications ensures that services are not restarted too frequently\.

Finally, `tomcat::deploy` runs `tomcat::context`, which updates the web app context configuration file if you have changed the back end database\. 