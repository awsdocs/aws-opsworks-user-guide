# Recipes<a name="workingcookbook-installingcustom-components-recipes"></a>

Recipes are Ruby applications that define a system's configuration\. They install packages, create configuration files from templates, execute shell commands, create files and directories, and so on\. You typically have AWS OpsWorks Stacks execute recipes automatically when a [lifecycle event](workingcookbook-events.md) occurs on the instance but you can also run them explicitly at any time by using the [Execute Recipes stack command](workingcookbook-executing.md)\. For more information, see [About Recipes](http://docs.chef.io/recipes.html)\.

A recipe typically consists largely of a series of *resources*, each of which represents the desired state of an aspect of the system\. Each resource includes a set of attributes that define the desired state and specify what action is to be taken\. Chef associates each resource with an appropriate *provider* that performs the action\. For more information, see [Resources and Providers Reference](https://docs.chef.io/resource.html)\.

A `package` resource helps you manage software packages on Linux instances\. The following example installs the Apache package\.

```
...
package 'apache2' do
  case node[:platform]
  when 'centos','redhat','fedora','amazon'
    package_name 'httpd'
  when 'debian','ubuntu'
    package_name 'apache2'
  end
  action :install
end
...
```

Chef uses the appropriate package provider for the platform\. Resource attributes are often just assigned a value, but you can use Ruby logical operations to perform conditional assignments\. The example uses a `case` operator, which uses `node[:platform]` to identify the instance's operating system and sets the `package_name` attribute accordingly\. You can insert attributes into a recipe by using the standard Chef node syntax and Chef replaces it with the associated value\. You can use any attribute in the node object, not just your cookbook's attributes\.

After determining the appropriate package name, the code segment ends with an `install` action, which installs the package\. Other actions for this resource include `upgrade` and `remove`\. For more information, see [package](https://docs.chef.io/chef/resources.html#id150)\.

It is often useful to break complex installation and configuration tasks into one or more subtasks, each implemented as a separate recipe, and have your primary recipe run them at the appropriate time\. The following example shows the line of code that follows the preceding example:

```
include_recipe 'apache2::service'
```

To have a recipe execute a child recipe, use the `include_recipe` keyword, followed by the recipe name\. Recipes are identified by using the standard Chef `CookbookName::RecipeName` syntax, where `RecipeName` omits the `.rb` extension\.

**Note**  
An `include_recipe` statement effectively executes the recipe at that point in the primary recipe\. However, what actually happens is that Chef replaces each `include_recipe` statement with the specified recipe's code before it executes the primary recipe\.

A `directory` resource represents a directory, such as the one that is to contain a package's files\. The following `default.rb` resource creates a Linux log directory\.

```
directory node[:apache][:log_dir] do
    mode 0755
    action :create
end
```

The log directory is defined in one of the cookbook's attribute files\. The resource specifies the directory's mode as 0755, and uses a `create` action to create the directory\. For more information, see [directory](https://docs.chef.io/chef/resources.html#directory)\. You can also use this resource with Windows instances\.

The `execute` resource represents commands, such as shell commands or scripts\. The following example generates module\.load files\.

```
execute 'generate-module-list' do
  if node[:kernel][:machine] == 'x86_64'
    libdir = 'lib64'
  else
    libdir = 'lib'
  end
  command "/usr/local/bin/apache2_module_conf_generate.pl /usr/#{libdir}/httpd/modules /etc/httpd/mods-available"
  action :run
end
```

The resource first determines the CPU type\. `[:kernel][:machine]` is another of the automatic attributes that Chef generates to represent various system properties, the CPU type in this case\. It then specifies the command, a Perl script and uses a `run` action to run the script, which generates the module\.load files\. For more information, see [execute](https://docs.chef.io/chef/resources.html#execute)\.

A `template` resource represents a file—typically a configuration file—that is to be generated from one of the cookbook's template files\. The following example creates an `httpd.conf` configuration file from the `apache2.conf.erb` template that was discussed in [Templates](workingcookbook-installingcustom-components-templates.md)\.

```
template 'apache2.conf' do
  case node[:platform]
  when 'centos','redhat','fedora','amazon'
    path "#{node[:apache][:dir]}/conf/httpd.conf"
  when 'debian','ubuntu'
    path "#{node[:apache][:dir]}/apache2.conf"
  end
  source 'apache2.conf.erb'
  owner 'root'
  group 'root'
  mode 0644
  notifies :restart, resources(:service => 'apache2')
end
```

The resource determines the generated file's name and location based on the instance's operating system\. It then specifies `apache2.conf.erb` as the template to be used to generate the file and sets the file's owner, group, and mode\. It runs the `notify` action to notify the `service` resource that represents the Apache server to restart the server\. For more information, see [template](https://docs.chef.io/chef/resources.html#template)\.