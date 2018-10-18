# Recipe Structure<a name="cookbooks-101-basics-structure"></a>

A cookbook is primarily a set of *recipes*, which can perform a wide variety of tasks on an instance\. To clarify how to implement recipes, it's useful to look at a simple example\. The following is the setup recipe for the built\-in [HAProxy layer](layers-haproxy.md)\. Just focus on the overall structure at this point and don't worry too much about the details; they will be covered in the subsequent examples\.

```
package 'haproxy' do
  action :install
end

if platform?('debian','ubuntu')
  template '/etc/default/haproxy' do
    source 'haproxy-default.erb'
    owner 'root'
    group 'root'
    mode 0644
  end
end

include_recipe 'haproxy::service'

service 'haproxy' do
  action [:enable, :start]
end

template '/etc/haproxy/haproxy.cfg' do
  source 'haproxy.cfg.erb'
  owner 'root'
  group 'root'
  mode 0644
  notifies :restart, "service[haproxy]"
end
```

**Note**  
For this and other examples of working recipes and related files, see the [AWS OpsWorks Stacks built\-in recipes](https://github.com/aws/opsworks-cookbooks)\.

The example highlights the key recipe elements, which are described in the following sections\.

**Topics**
+ [Resources](#cookbooks-101-basics-structure-resources)
+ [Flow Control](#cookbooks-101-basics-structure-ruby)
+ [Included Recipes](#cookbooks-101-basics-structure-include)

## Resources<a name="cookbooks-101-basics-structure-resources"></a>

Recipes consist largely of a set of Chef *resources*\. Each one specifies a particular aspect of the instance's final state, such as a package to be installed or a service to be started\. The example has four resources:
+ A `package` resource, which represents an installed package, an [HAProxy server](http://haproxy.1wt.eu/) for this example\.
+ A `service` resource, which represents a service, the HAProxy service for this example\.
+ Two `template` resources, which represent files that are to be created from a specified template, two HAProxy configuration files for this example\.

Resources provide a declarative way to specify the instance state\. Behind the scenes, each resource has an associated *provider* that performs the required tasks, such as installing packages, creating and configuring directories, starting services, and so on\. If the details of the task depend on the particular operating system, the resource has multiple providers and uses the appropriate one for the system\. For example, on a Red Hat Linux system the `package` provider uses `yum` to install packages\. On a Ubuntu Linux system, the `package` provider uses `apt-get`\.

You implement a resource as a Ruby code block with the following general format\.

```
resource_type "resource_name" do
  attribute1 'value1'
  attribute2 'value2'
  ...
  action :action_name
  notifies : action 'resource'
end
```

The elements are:

**Resource type**  
\(Required\) The example includes three resource types, `package`, `service`, and `template`\.

**Resource name**  
\(Required\) The name identifies the particular resource and is sometimes used as a default value for one of the attributes\. In the example, `package` represents a package resource named `haproxy` and the first `template` resource represents a configuration file named `/etc/default/haproxy`\.

**Attributes**  
\(Optional\) Attributes specify the resource configuration and vary depending on the resource type and how you want to configure the resource\.  
+ The example's `template` resources explicitly define a set of attributes that specify the created file's source, owner, group, and mode\. 
+ The example's `package` and `service` resources do not explicitly define any attributes\.

  The resource name is typically the default value for a required attribute and is sometimes all that is needed\. For example, the resource name is the default value for the `package` resource's `package_name` attribute, which is the only required attribute\.
There are also some specialized attributes called guard attributes, which specify when the resource provider is to take action\. For example, the `only_if` attribute directs the resource provider to take action only if a specified condition is met\. The HAProxy recipe does not use guard attributes, but they are used by several of the following examples\.

**Actions and Notifications**  
\(Optional\) Actions and notifications specify what tasks the provider is to perform\.  
+ `action` directs the provider to take a specified action, such as install or create\.

  Each resource has a set of actions that depend on the particular resource, one of which is the default action\. In the example, the `package` resource's action is `install`, which directs the provider to install the package\. The first `template` resource has no `action` element, so the provider takes the default `create` action\.
+ `notifies` directs another resource's provider to perform an action, but only if the resource's state has changed\.

  `notifies` is typically used with resources such as `template` and `file` to perform tasks such as restarting a service after modifying a configuration file\. Resources do not have default notifications\. If you want a notification, the resource must have an explicit `notifies` element\. In the HAProxy recipe, the second `template` resource notifies the haproxy `service` resource to restart the HAProxy service if the associated configuration file has changed\. 

Resources sometimes depend on operating system\.
+ Some resources can be used only on Linux or Windows systems\.

  For example, [package](https://docs.chef.io/chef/resources.html#package) installs packages on Linux systems and [windows\_package](https://docs.chef.io/chef/resources.html#windows-package) installs packages on Windows systems\.
+ Some resources can be used with any operating system, but have attributes that are specific to a particular system\.

  For example, the [file](https://docs.chef.io/chef/resources.html#file) resource can be used on either Linux or Windows systems, but has separate sets of attributes for configuring permissions\.

For descriptions of the standard resources, including the available attributes, actions, and notifications for each resource, see [About Resources and Providers](https://docs.chef.io/resource.html)\. 

## Flow Control<a name="cookbooks-101-basics-structure-ruby"></a>

Because recipes are Ruby applications, you can use Ruby control structures to incorporate flow control into a recipe\. For example, you can use Ruby conditional logic to have the recipe behave differently on different systems\. The HAProxy recipe includes an `if` block that uses a `template` resource to create a configuration file, but only if the recipe is running on a Debian or Ubuntu system\. 

Another common scenario is using a loop to execute a resource multiple times with different attribute settings\. For example, you can create a set of directories by using a loop to execute a `directory` resource multiple times with different directory names\.

**Note**  
If you aren't familiar with Ruby, see [Just Enough Ruby for Chef](https://docs.chef.io/just_enough_ruby_for_chef.html), which covers what you need to know for most recipes\.

## Included Recipes<a name="cookbooks-101-basics-structure-include"></a>

`include_recipe` includes other recipes in your code, which allows you to modularize your recipes and reuse the same code in multiple recipes\. When you run the host recipe, Chef replaces each `include_recipe` element with the specified recipe's code before it executes the host recipe\. You identify an included recipe by using the standard Chef `cookbook_name::recipe_name` syntax, where `recipe_name` omits the `.rb` extension\. The example includes one recipe, `haproxy::service`, which represents the HAProxy service\. 

**Note**  
If you use `include_recipe` in recipes running on Chef 11\.10 and later to include a recipe from another cookbook, you must use a `depends` statement to declare the dependency in the cookbook's `metadata.rb` file\. For more information, see [Implementing Recipes: Chef 11\.10](workingcookbook-chef11-10.md)\.