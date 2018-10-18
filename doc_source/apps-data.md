# Passing Data to Applications<a name="apps-data"></a>

It is often useful to pass data such as key\-value pairs to an application on the server\. To do so, use [custom JSON](workingstacks-json.md) to add the data to the stack\. AWS OpsWorks Stacks adds the data to each instance's node object for each lifecycle event\. 

Note, however, that although recipes can get the custom JSON data from the node object by using Chef attributes, applications cannot\. One approach to getting custom JSON data to one or more applications is to implement a custom recipe that extracts the data from the `node` object and writes it to a file that the application can read\. The example in this topic shows how to write the data to a YAML file, but you can use the same basic approach for other formats, such as JSON or XML\.

To pass key\-value data to the stack's instances, add custom JSON like the following to the stack\. For more information about how to add custom JSON to a stack, see [Using Custom JSON](workingstacks-json.md)\.

```
{
  "my_app_data": {
    "app1": {
      "key1": "value1",
      "key2": "value2",
      "key3": "value3"
    },
    "app2": {
      "key1": "value1",
      "key2": "value2",
      "key3": "value3"
    }
  }
}
```

The example assumes that you have two apps whose short names are `app1` and `app2`, each of which has three data values\. The accompanying recipe assumes that you use the apps' short names to identify the associated data; the other names are arbitrary\. For more information on app short names, see [Settings](workingapps-creating.md#workingapps-creating-settings)\.

The recipe in the following example shows how to extract the data for each app from the `deploy` attributes and put it in a `.yml` file\. The recipe assumes that your custom JSON contains data for each app\.

```
node[:deploy].each do |app, deploy|
  file File.join(deploy[:deploy_to], 'shared', 'config', 'app_data.yml') do
    content YAML.dump(node[:my_app_data][app].to_hash)
  end
end
```

The `deploy` attributes contain an attribute for each app, named with the app's short name\. Each app attribute contains a set of attributes that represent a variety of information about the app\. This example uses the app's deployment directory, which is represented by the `[:deploy][:app_short_name][:deploy_to]` attribute\. For more information on `[:deploy]`, see [deploy Attributes](attributes-json-deploy.md)\.

For each app in `deploy`, the recipe does the following:

1. Creates a file named `app_data.yml` in the `shared/config` subdirectory of the application's `[:deploy_to]`directory\.

   For more information on how AWS OpsWorks Stacks installs apps, see [Deploy Recipes](create-custom-deploy.md)\.

1. Converts the app's custom JSON values to YAML and writes the formatted data to `app_data.yml`\.

**To pass data to an app**

1. Add an app to the stack and note its short name\. For more information, see [Adding Apps](workingapps-creating.md)\.

1. Add custom JSON with the app's data to the `deploy` attributes, as described earlier\. For more information on how to add custom JSON to a stack, see [Using Custom JSON](workingstacks-json.md)\.

1. Create a cookbook and add a recipe to it with code based on the previous example, modified as needed for the attribute names that you used in the custom JSON\. For more information on how to create cookbooks and recipes, see [Cookbooks and Recipes](workingcookbook.md)\. If you already have custom cookbooks for this stack, you could also add the recipe to an existing cookbook, or even add the code to an existing Deploy recipe\.

1. Install the cookbook on your stack\. For more information, see [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)\.

1. Assign the recipe to the app server layer's Deploy lifecycle event\. AWS OpsWorks Stacks will then run the recipe on each new instance, after it has booted\. For more information, see [Executing Recipes](workingcookbook-executing.md)\.

1. Deploy the app, which also installs stack configuration and deployment attributes that now contain your data\.

**Note**  
If the data files must be in place before the app is deployed, you can also assign the recipe to the layer's Setup lifecycle event, which occurs once, right after the instance finishes booting\. However, AWS OpsWorks Stacks will not have created the deployment directories yet, so your recipe should create the required directories explicitly prior to creating the data file\. The following example explicitly creates the app's `/shared/config` directory, and then creates a data file in that directory\.  

```
node[:deploy].each do |app, deploy|

 directory "#{deploy[:deploy_to]}/shared/config" do
      owner "deploy"
      group "www-data"
      mode 0774
      recursive true
      action :create
    end

  file File.join(deploy[:deploy_to], 'shared', 'config', 'app_data.yml') do
    content YAML.dump(node[:my_app_data][app].to_hash)
  end
end
```

To load the data, you can use something like the following [Sinatra](http://www.sinatrarb.com/) code:

```
#!/usr/bin/env ruby
# encoding: UTF-8
require 'sinatra'
require 'yaml'

get '/' do
  YAML.load(File.read(File.join('..', '..', 'shared', 'config', 'app_data.yml')))
End
```

You can update the app's data values at any time by updating the custom JSON, as follows\.

**To update the app data**

1. Edit the custom JSON to update the data values\.

1. Deploy the app again, which directs AWS OpsWorks Stacks to run the Deploy recipes on the stack's instances\. The recipes will use attributes from the updated stack configuration and deployment attributes, so your custom recipe will update the data files with the current values\.