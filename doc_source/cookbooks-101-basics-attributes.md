# Example 5: Using Attributes<a name="cookbooks-101-basics-attributes"></a>

The recipes in the preceding sections used hard\-coded values for everything other than the platform\. This approach can be inconvenient if, for example, you want to use the same value in more than one recipe\. You can define values separately from recipes by including an attribute file in your cookbook\.

An attribute file is a Ruby application that assigns values to one or more attributes\. It must be in the cookbook's `attributes` folder\. Chef incorporates the attributes into the node object and any recipe can use the attribute values by referencing the attribute\. This topic shows how to modify the recipe from [Iteration](cookbooks-101-basics-ruby.md#cookbooks-101-basics-ruby-iteration) to use attributes\. Here's the original recipe for reference\.

```
[ "/srv/www/config", "/srv/www/shared" ].each do |path|
  directory path do
    mode 0755
    owner 'root'
    group 'root'
    recursive true
    action :create
  end
end
```

The following defines attributes for the subdirectory name, mode, owner, and group values\.

```
default['createdir']['shared_dir'] = 'shared'
default['createdir']['config_dir'] = 'config'
default['createdir']['mode'] = 0755
default['createdir']['owner'] = 'root'
default['createdir']['group'] = 'root'
```

Note the following:
+ Each definition starts with an *attribute type*\.

  If an attribute is defined more than once—perhaps in different attribute files—the attribute type specifies the attribute's precedence, which determines which definition is incorporated into the node object\. For more information, see [Attribute Precedence](workingcookbook-attributes-precedence.md)\. All the definitions in this example have the `default` attribute type, which is the usual type for this purpose\.
+ The attributes have nested names\.

  The node object is basically a hash table that can be nested arbitrarily deeply, so attribute names can be and commonly are nested\. This attribute file follows a standard practice of using a nested name with the cookbook name, `createdir`, as the first element\.

The reason for using createdir as the attribute's first element is that when you do a Chef run, Chef incorporates the attributes from every cookbook into the node object\. With AWS OpsWorks Stacks, the node object includes a large number of attributes from the [built\-in cookbooks](https://github.com/aws/opsworks-cookbooks) in addition to any attributes that you define\. Including the cookbook name in the attribute name reduces the risk of a name collision with attributes from another cookbook, especially if your attribute has a name like `port` or `user`\. Don't name an attribute something like [`[:apache2][:user]`](attributes-recipes-apache.md#attributes-recipes-apache-user), for example, unless you want to override that attribute's value\. For more information, see [Using Custom Cookbook Attributes](workingcookbook-cookbook-attributes.md)\.

The following example shows the original recipe using attributes instead of hard\-coded values\.

```
[ "/srv/www/#{node['createdir']['shared_dir']}", "/srv/www/#{node['createdir']['config_dir']}" ].each do |path|
  directory path do
    mode node['createdir']['mode']
    owner node['createdir']['owner']
    group node['createdir']['group']
    recursive true
    action :create
  end
end
```

**Note**  
If you want to incorporate an attribute value into a string, wrap it with `#{}`\. In the preceding example, `#{node['createdir']['shared_dir']}` appends "shared" to "/srv/www/"\.

**To run the recipe**

1. Run `kitchen destroy` to start with a clean instance\.

1. Replace the code in `recipes/default.rb` with the preceding recipe example\.

1. Create a subdirectory of `createdir` named `attributes` and add a file named `default.rb` that contains the attribute definitions\.

1. Edit `.kitchen.yml` to remove CentOS from the platforms list\.

1. Run `kitchen converge` and then log in to the instance and verify that `/srv/www/shared` and `/srv/www/config` are there\.

**Note**  
With AWS OpsWorks Stacks, defining values as attributes provides an additional benefit; you can use [custom JSON](workingstacks-json.md) to override those values on a per\-stack or even per\-deployment basis\. This can be useful for a variety of purposes, including the following:  
You can customize the behavior of your recipes, such as configuration settings or user names, without having to modify the cookbook\.  
You can, for example, use the same cookbook for different stacks and use custom JSON to specify key configuration settings for a particular stack\. This saves you the time and effort required to modify the cookbook or use a different cookbook for each stack\.
You don't have to put potentially sensitive information such as database passwords in your cookbook repository\.  
You can instead use an attribute to define a default value and then use custom JSON to override that value with the real one\.
For more information on how to use custom JSON to override attributes, see [Overriding Attributes](workingcookbook-attributes.md)\.

The attribute file is named `default.rb` because it is a Ruby application, if a rather simple one\. That means you can, for example, use conditional logic to specify attribute values based on the operating system\. In [Conditional Logic](cookbooks-101-basics-ruby.md#cookbooks-101-basics-ruby-conditional), you specified a different subdirectory name for different Linux families in the recipe\. With an attribute file, you can instead put the conditional logic in the attribute file\.

The following attribute file uses `value_for_platform` to specify a different `['shared_dir']` attribute value, depending on the operating system\. For other conditions, you can use Ruby `if-elsif-else` logic or a `case` statement\.

```
data_dir = value_for_platform(
  "centos" => { "default" => "shared" },
  "ubuntu" => { "default" => "data" },
  "default" => "user_data"
)
default['createdir']['shared_dir'] = data_dir
default['createdir']['config_dir'] = "config"
default['createdir']['mode'] = 0755
default['createdir']['owner'] = 'root'
default['createdir']['group'] = 'root'
```

**To run the recipe**

1. Run `kitchen destroy` to start with a fresh instance\.

1. Replace the code in `attributes/default.rb` with the preceding example\.

1. Edit `.kitchen.yml` to add a CentOS platform to the platforms section, as described in [Conditional Logic](cookbooks-101-basics-ruby.md#cookbooks-101-basics-ruby-conditional)\.

1. Run `kitchen converge`, and then log in to the instances to verify that the directories are there\.

When you are finished, run `kitchen destroy` to terminate the instance\. The next example uses a new cookbook\.