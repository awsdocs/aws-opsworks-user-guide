# Example 8: Managing Services<a name="cookbooks-101-basics-services"></a>

Packages such as application servers typically have an associated service that must be started, stopped, restarted, and so on\. For example, you need to start the Tomcat service after installing the package or after the instance finishes booting, and restart the service each time you modify the configuration file\. This topic discusses the basics of how to manage a service on a Linux instance, using a Tomcat application server as an example\. The service resource works much the same way on Windows instances, although there are some differences in detail\. For more information, see [https://docs.chef.io/chef/resources.html#service](https://docs.chef.io/chef/resources.html#service)\.

**Note**  
The example does a very minimal Tomcat installation, just enough to demonstrate the basics of how to use a `service` resource\. For an example of how to implement recipes for a more functional Tomcat server, see [Creating a Custom Tomcat Server Layer](create-custom.md)\.

**Topics**
+ [Defining and Starting a Service](#cookbooks-101-basics-services-service)
+ [Using notifies to Start or Restart a Service](#cookbooks-101-basics-services-notifies)

## Defining and Starting a Service<a name="cookbooks-101-basics-services-service"></a>

This section shows the basics of how to define and start a service\.

**To get started**

1. In the `opsworks_cookbooks` directory, create a directory named `tomcat` and navigate to it\.

1. Add a `metadata.rb` file to `tomcat` with the following content\.

   ```
   name "tomcat"
   version "0.1.0"
   ```

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md), and remove CentOS from the `platforms` list\.

1. Add a `recipes` subdirectory to `tomcat`\.

You use a [https://docs.chef.io/chef/resources.html#service](https://docs.chef.io/chef/resources.html#service) resource to manage a service\. The following default recipe installs Tomcat and starts the service\.

```
execute "install_updates" do
  command "apt-get update"
end

package "tomcat7" do
    action :install
end

include_recipe 'tomcat::service'

service 'tomcat' do
  action :start
end
```

The recipe does the following:
+ The `execute` resource runs `apt-get update` to install the current system updates\.

  For the Ubuntu 12\.04 LTS instance used in this example, you must install the updates before installing Tomcat\. Other systems might have different requirements\.
+ The `package` resource installs Tomcat 7\.
+ The included`tomcat::service` recipe defines the service and is discussed later\.
+ The `service` resource starts the Tomcat service\.

  You can also use this resource to issue other commands, such as stopping and restarting the service\.

The following example shows the `tomcat::service` recipe\.

```
service 'tomcat' do
  service_name "tomcat7"
  supports :restart => true, :reload => false, :status => true
  action :nothing
end
```

This recipe creates the Tomcat service definition as follows: 
+ The resource name, `tomcat`, is used by other recipes to reference the service\.

  For example, `default.rb` references `tomcat` to start the service\.
+ The `service_name` resource specifies the service name\. 

  When you list the services on the instance, the Tomcat service will be named tomcat7\.
+ `supports` specifies how Chef manages the service's `restart`, `reload`, and `status` commands\.
  + `true` indicates that Chef can use the init script or other service provider to run the command\.
  + `false` indicates that Chef must attempt to run the command by other means\.

Notice that `action` is set to `:nothing`, which directs the resource to take no action\. The service resource does support actions such as `start` and `restart`\. However, this cookbook follows a standard practice of using a service definition that takes no action and starting or restarting the service elsewhere\. Each recipe that starts or restarts a service must first define it, so the simplest approach is to put the service definition in a separate recipe and include it in other recipes as needed\.

**Note**  
For simplicity, the default recipe for this example uses a `service` resource to start the service after running the service definition\. A production implementation typically starts or restarts a service by using `notifies`, as discussed later\.

**To run the recipe**

1. Create a `default.rb` file that contains the default recipe example and save it to `recipes`\.

1. Create a `service.rb` file that contains the service definition example and save it to `recipes`\.

1. Run `kitchen converge`, then log in to the instance and run the following command to verify that the service is running\.

   ```
   sudo service tomcat7 status
   ```

**Note**  
If you were running `service.rb` separately from `default.rb`, you would have to edit `.kitchen.yml` to add `tomcat::service` to the run list\. However, when you include a recipe, its code is incorporated into the parent recipe before the recipe is executed\. `service.rb` is therefore basically a part of `default.rb` and doesn't require a separate run list entry\.

## Using notifies to Start or Restart a Service<a name="cookbooks-101-basics-services-notifies"></a>

Production implementations typically do not use `service` to start or restart a service\. Instead, they add `notifies` to any of several resources\. For example, if you want to restart the service after modifying a configuration file, you include `notifies` in the associated `template` resource\. Using `notifies` has the following advantages over using a `service` resource to explicitly restart the service\. 
+ The `notifies` element restarts the service only if the associated configuration file has changed, so there's no risk of causing an unnecessary service restart\. 
+ Chef restarts the service at most once at the end of each run, regardless of how many `notifies` the run contains\.

  For example, Chef run might include multiple template resources, each of which modifies a different configuration file and requires a service restart if the file has changed\. However, you typically want to restart the service only once, at the end of the Chef run\. Otherwise, you might attempt to restart a service that is not yet fully operational from an earlier restart, which can lead to errors\.

This example modifies `tomcat::default` to include a `template` resource that uses `notifies` to restart the service\. A realistic example would use a template resource that creates a customized version of one of the Tomcat configuration files, but those are rather long and complex\. For simplicity, the example just uses the template resource from [Creating a File from a Template](cookbooks-101-basics-files.md#cookbooks-101-basics-files-template)\. It doesn't have anything to do with Tomcat, but it provides a simple way to show how to use `notifies`\. For an example of how to use templates to create Tomcat configuration files, see [Setup Recipes](create-custom-setup.md)\.

**To set up the cookbook**

1. Add a `templates` subdirectory to `tomcat` and a `default` subdirectory to `templates`\.

1. Copy the `example_data.json.erb` template from the `createfile` cookbook to the `templates/default` directory\.

1. Add an `attributes` subdirectory to `tomcat`\.

1. Copy the `default.rb` attribute file from the `createfile` cookbook to the `attributes` directory\.

The following recipe uses `notifies` to restart the Tomcat service\.

```
execute "install_updates" do
  command "apt-get update"
end

package "tomcat7" do
    action :install
end

include_recipe 'tomcat::service'

service 'tomcat' do
  action :enable
end

directory "/srv/www/shared" do
  mode 0755
  owner 'root'
  group 'root'
  recursive true
  action :create
end

template "/srv/www/shared/example_data.json" do
  source "example_data.json.erb"
  mode 0644
  variables(
    :a_boolean_var => true,
    :a_string_var => "some string"
  )
  only_if {node['createfile']['install_file']}
  notifies :restart, resources(:service => 'tomcat')
end
```

The example merges the recipe from [Creating a File from a Template](cookbooks-101-basics-files.md#cookbooks-101-basics-files-template) into the recipe from the preceding section, with two significant changes:
+ The `service` resource is still there, but it now serves a somewhat different purpose\.

  The `:enable` action enables the Tomcat service at boot\.
+ The template resource now includes `notifies`, which restarts the Tomcat service if `example_data.json` has changed\.

  This ensures that the service is started when Tomcat is first installed and restarted after every configuration change\.

**To run the recipe**

1. Run `kitchen destroy` to start with a clean instance\.

1. Replace the code in `default.rb` with the preceding example\.

1. Run `kitchen converge`, then log in to the instance and verify that the service is running\.

**Note**  
If you want to restart a service but the recipe doesn't include a resource such as `template` that supports `notifies`, you can instead use a dummy `execute` resource\. For example  

```
execute 'trigger tomcat service restart' do
  command 'bin/true'
  notifies :restart, resources(:service => 'tomcat')
end
```
The `execute` resource must have a `command` attribute, even if you are using the resource only as a way to run `notifies`\. This example gets around that requirement by running `/bin/true`, which is a shell command that simply returns a success code\.