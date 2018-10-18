# Example 4: Adding Flow Control<a name="cookbooks-101-basics-ruby"></a>

Some recipes are just a series of Chef resources\. In that case, when you run the recipe, it simply executes each of the resource providers in sequence\. However, it's often useful to have a more sophisticated execution path\. The following are two common scenarios:
+ You want a recipe to execute the same resource multiple times with different attribute settings\.
+ You want to use different attribute settings on different operating systems\.

You can address scenarios such as these by incorporating Ruby control structures into the recipe\. This section shows how to modify the recipe from [Example 3: Creating Directories](cookbooks-101-basics-directories.md) to address both scenarios\.

**Topics**
+ [Iteration](#cookbooks-101-basics-ruby-iteration)
+ [Conditional Logic](#cookbooks-101-basics-ruby-conditional)

## Iteration<a name="cookbooks-101-basics-ruby-iteration"></a>

[Example 3: Creating Directories](cookbooks-101-basics-directories.md) showed how to use a `directory` resource to create a directory or chain of directories\. However, suppose that you want to create two separate directories, `/srv/www/config` and `/srv/www/shared`\. You could implement a separate directory resource for each directory, but that approach can get cumbersome if you want to create very many directories\. The following recipe shows a simpler way to handle the task\. 

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

Instead of using a separate directory resource for each subdirectory, the recipe uses a string collection that contains the subdirectory paths\. The Ruby `each` method executes the resource once for each collection element, starting with the first one\. The element's value is represented in the resource by the `path` variable, which in this case represents the directory path\. You can easily adapt this example to create any number of subdirectories\.

**To run the recipe**

1. Stay in `createdir` directory; you'll be using that cookbook for the next several examples\. 

1. If you haven't done so already, run `kitchen destroy` so you are starting with a clean instance\. 

1. Replace the code in `default.rb` with the example and run `kitchen converge`\.

1. Log in to the instance; you will see the newly created directories under `/srv`\.

You can use a hash table to specify two values for each iteration\. The following recipe creates `/srv/www/config` and `/srv/www/shared`, each with a different mode\.

```
{ "/srv/www/config" => 0644, "/srv/www/shared" => 0755 }.each do |path, mode_value|
  directory path do
    mode mode_value
    owner 'root'
    group 'root'
    recursive true
    action :create
  end
end
```

**To run the recipe**

1. If you haven't done so already, run `kitchen destroy` so you are starting with a clean instance\. 

1. Replace the code in `default.rb` with the example and run `kitchen converge`\.

1. Log in to the instance; you will see the newly created directories under `/srv` with the specified modes\.

**Note**  
AWS OpsWorks Stacks recipes commonly use this approach to extract values from the [stack configuration and deployment JSON](workingcookbook-json.md)—which is basically a large hash table—and insert them in a resource\. For an example, see [Deploy Recipes](create-custom-deploy.md)\.

## Conditional Logic<a name="cookbooks-101-basics-ruby-conditional"></a>

You can also use Ruby conditional logic to create multiple execution branches\. The following recipe uses `if-elsif-else` logic to extend the previous example so that it creates a subdirectory named `/srv/www/shared`, but only on Debian and Ubuntu systems\. For all other systems, it logs an error message that is displayed in the Test Kitchen output\.

```
if platform?("debian", "ubuntu")
  directory "/srv/www/shared" do
    mode 0755
    owner 'root'
    group 'root'
    recursive true
    action :create
  end
else
  log "Unsupported system"
end
```

**To run the example recipe**

1. If your instance is still up, run `kitchen destroy` to shut it down\.

1. Replace the code in `default.rb` with the example code\.

1. Edit `.kitchen.yml` to add a CentOS 6\.4 system to the platform list\. The file's `platforms` section should now look like\.

   ```
   ...
   platforms:
     - name: ubuntu-12.04
     - name: centos-6.4
   ...
   ```

1. Run `kitchen converge`, which will create an instance and run the recipes for each platform in `.kitchen.yml`, in sequence\. 
**Note**  
If you want to converge just one instance, add the instance name as a parameter\. For example, to converge the recipe only on the Ubuntu platform, run `kitchen converge default-ubuntu-1204`\. If you forget the platform names, just run `kitchen list`\.

You should see your log message in the CentOS part of the Test Kitchen output, which will look something like the following:

```
...
Converging 1 resources
Recipe: createdir::default
* log[Unsupported system] action write[2014-06-23T19:10:30+00:00] INFO: Processing log[Unsupported system] action write (createdir::default line 12)
[2014-06-23T19:10:30+00:00] INFO: Unsupported system
       
[2014-06-23T19:10:30+00:00] INFO: Chef Run complete in 0.004972162 seconds
```

You can now log in to the instances and verify that the directories were or were not created\. However, you can't simply run `kitchen login` now\. You must specify which instance by appending the platform name, for example, `kitchen login default-ubuntu-1204` \. 

**Note**  
If a Test Kitchen command takes an instance name, you don't need to type the complete name\. Test Kitchen treats an instance name as a Ruby regular expression, so you just need enough characters to provide a unique match\. For example, you can converge just the Ubuntu instance by running `kitchen converge ub` or log in to the CentOS instance by running `kitchen login 64`\.

The question you probably have at this point is how the recipe knows which platform it is running on\. Chef runs a tool called [Ohai](https://docs.chef.io/ohai.html) for every run that collects system data, including the platform, and represents it as a set of attributes in a structure called the *node object*\. The Chef `platform?` method compares the systems in parentheses against the Ohai platform value, and returns true if one of them matches\.

You can reference the value of a node attribute directly in your code by using `node['attribute_name']`\. The platform value, for example, is represented by `node['platform']`\. You could, for example, have written the preceding example as follows\.

```
if node[:platform] == 'debian' or node[:platform] == 'ubuntu'
  directory "/srv/www/shared" do
    mode 0755
    owner 'root'
    group 'root'
    recursive true
    action :create
  end
else
  log "Unsupported system"
end
```

A common reason for including conditional logic in a recipe is to accommodate the fact that different Linux families sometimes use different names for packages, directories, and so on\. For example, the Apache package name is `httpd` on CentOS systems and `apache2` on Ubuntu systems\.

If you just need a different string for different systems, the Chef [http://docs.chef.io/dsl_recipe.html#value-for-platform](http://docs.chef.io/dsl_recipe.html#value-for-platform) method is a simpler solution than `if-elsif-else`\. The following recipe creates a `/srv/www/shared` directory on CentOS systems, a `/srv/www/data` directory on Ubuntu systems, and `/srv/www/config` on all others\.

```
data_dir = value_for_platform(
  "centos" => { "default" => "/srv/www/shared" },
  "ubuntu" => { "default" => "/srv/www/data" },
  "default" => "/srv/www/config"
)
directory data_dir do
  mode 0755
  owner 'root'
  group 'root'
  recursive true
  action :create
end
```

`value_for_platform` assigns the appropriate path to `data_dir` and the `directory` resource uses that value to create the directory\.

**To run the example recipe**

1. If your instance is still up, run `kitchen destroy` to shut it down\.

1. Replace the code in `default.rb` with the example code\.

1. Run `kitchen converge` and then login to each instance to verify that the appropriate directories are present\.