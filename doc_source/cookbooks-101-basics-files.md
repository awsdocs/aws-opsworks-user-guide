# Example 6: Creating Files<a name="cookbooks-101-basics-files"></a>

After you have created directories, you often need to populate them with configuration files, data files, and so on\. This topic shows two ways to install files on an instance\.

**Topics**
+ [Installing a File from a Cookbook](#cookbooks-101-basics-files-cookbook_file)
+ [Creating a File from a Template](#cookbooks-101-basics-files-template)

## Installing a File from a Cookbook<a name="cookbooks-101-basics-files-cookbook_file"></a>

The simplest way to install a file on an instance is to use a [https://docs.chef.io/chef/resources.html#cookbook-file](https://docs.chef.io/chef/resources.html#cookbook-file) resource, which copies a file from the cookbook to a specified location on the instance for both Linux and Windows systems\. This example extends the recipe from [Example 3: Creating Directories](cookbooks-101-basics-directories.md) to add a data file to `/srv/www/shared` after the directory is created\. For reference, here is the original recipe\.

```
directory "/srv/www/shared" do
  mode 0755
  owner 'root'
  group 'root'
  recursive true
  action :create
end
```

**To set up the cookbook**

1. Inside the `opsworks_cookbooks` directory, create a directory named `createfile` and navigate to it\.

1. Add a `metadata.rb` file to `createfile` with the following content\.

   ```
   name "createfile"
   version "0.1.0"
   ```

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md), and remove CentOS from the `platforms` list\.

1. Add a `recipes` subdirectory to `createfile`\.

The file to be installed contains the following JSON data\.

```
{
  "my_name" : "myname",
  "your_name" : "yourname",
  "a_number" : 42,
  "a_boolean" : true
}
```

**To set up the data file**

1. Add a `files` subdirectory to `createfile` and a `default` subdirectory to `files`\. Any file that you install with `cookbook_file` must be in a subdirectory of `files`, such as `files/default` in this example\. 
**Note**  
If you want to specify different files for different systems, you can put each system\-specific file in a subfolder named for the system, such as `files/ubuntu`\. The `cookbook_file` resource copies the appropriate system\-specific file, if it exists, and otherwise uses the `default` file\. For more information, see [cookbook\_file](https://docs.chef.io/chef/resources.html#cookbook-file)\.

1. Create a file named `example_data.json` with the JSON from the preceding example and add it to `files/default`\.

The following recipe copies `example_data.json` to a specified location\. 

```
directory "/srv/www/shared" do
  mode 0755
  owner 'root'
  group 'root'
  recursive true
  action :create
end

cookbook_file "/srv/www/shared/example_data.json" do
  source "example_data.json"
  mode 0644
  action :create_if_missing
end
```

After the directory resource creates `/srv/www/shared`, the `cookbook_file` resource copies `example_data.json` to that directory and also sets the file's user, group, and mode\. 

**Note**  
The `cookbook_file` resource introduces a new action: `create_if_missing`\. You could also use a `create` action, but that overwrites an existing file\. If you don't want to overwrite anything, use `create_if_missing`, which installs `example_data.json` only if it does not already exist\.

**To run the recipe**

1. Run `kitchen destroy` to start with a fresh instance\.

1. Create a `default.rb` file that contains the preceding recipe and save it to `recipes`\.

1. Run `kitchen converge`, then log in to the instance to verify that `/srv/www/shared` contains`example_data.json`\.

## Creating a File from a Template<a name="cookbooks-101-basics-files-template"></a>

The `cookbook_file` resource is useful for some purposes, but it just installs whatever file you have in the cookbook\. A [https://docs.chef.io/chef/resources.html#template](https://docs.chef.io/chef/resources.html#template) resource provides a more flexible way to install a file on a Windows or Linux instance by creating it dynamically from a template\. You can then determine the details of the file's contents at runtime and change them as needed\. For example, you might want a configuration file to have a particular setting when you start the instance and modify the setting later when you add more instances to the stack\.

This example modifies the `createfile` cookbook to use a `template` resource to install a slightly modified version of `example_data.json`\.

Here's what the installed file will look like\.

```
{
  "my_name" : "myname",
  "your_name" : "yourname",
  "a_number" : 42,
  "a_boolean" : true,
  "a_string" : "some string",
  "platform" : "ubuntu"
}
```

Template resources are typically used in conjunction with attribute files, so the example uses one to define the following values\.

```
default['createfile']['my_name'] = 'myname'
default['createfile']['your_name'] = 'yourname'
default['createfile']['install_file'] = true
```

**To set up the cookbook**

1. Delete the `createfile` cookbook's `files` directory and its contents\.

1. Add an `attributes` subdirectory to `createfile` and add a `default.rb` file to `attributes` that contains the preceding attribute definitions\.

A template is a `.erb` file that is basically a copy of the final file, with some of the contents represented by placeholders\. When the `template` resource creates the file, it copies the template's contents to the specified file, and overwrites the placeholders with their assigned values\. Here's the template for `example_data.json`\.

```
{
  "my_name" : "<%= node['createfile']['my_name'] %>",
  "your_name" : "<%= node['createfile']['your_name'] %>",
  "a_number" : 42,
  "a_boolean" : <%= @a_boolean_var %>,
  "a_string" : "<%= @a_string_var %>",
  "platform" : "<%= node['platform'] %>"
}
```

The `<%=...%>` values are the placeholders\.
+ `<%=node[...]%>` represents a node attribute value\.

  For this example, the "your\_name" value is a placeholder that represents one of the attribute values from the cookbook's attribute file\.
+ `<%=@...%>` represents the value of a variable that is defined in the template resource, as discussed shortly\.

**To create the template file**

1. Add a `templates` subdirectory to the `createfile` cookbook and a `default` subdirectory to `templates`\.
**Note**  
The `templates` directory works much like the `files` directory\. You can put system\-specific templates in a subdirectory such as `ubuntu` that is named for the system\. The `template` resource uses the appropriate system\-specific template if it exists and otherwise uses the `default` template\.

1. Create a file named `example_data.json.erb` and put in the `templates/default` directory\. The template name is arbitrary, but you usually create it by appending `.erb` to the file name, including any extensions\. 

The following recipe uses a `template` resource to create `/srv/www/shared/example_data.json`\. 

```
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
end
```

The `template` resource creates `example_data.json` from a template and installs it in `/srv/www/shared`\.
+ The template name, `/srv/www/shared/example_data.json`, specifies the installed file's path and name\.
+ The `source` attribute specifies the template used to create the file\.
+ The `mode` attribute specifies the installed file's mode\.
+ The resource defines two variables, `a_boolean_var` and `a_string_var`\. 

  When the resource creates `example_data.json`, it overwrites the variable placeholders in the template with the corresponding values from the resource\. 
+ The `only_if` *guard* attribute directs the resource to create the file only if `['createfile']['install_file']` is set to `true`\.

**To run the recipe**

1. Run `kitchen destroy` to start with a fresh instance\.

1. Replace the code in `recipes/default.rb` with the preceding example\.

1. Run `kitchen converge`, then log in to the instance to verify that the file is in `/srv/www/shared` and has the correct content\.

When you are finished, run `kitchen destroy` to shut down the instance\. The next section uses a new cookbook\.