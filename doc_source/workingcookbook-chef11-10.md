# Implementing Recipes for Chef 11\.10 Stacks<a name="workingcookbook-chef11-10"></a>

Chef 11\.10 stacks provide the following advantages over Chef 11\.4 stacks:
+ Chef runs use Ruby 2\.0\.0, so your recipes can use the new Ruby syntax\.
+ Recipes can use Chef search and data bags\.

  Chef 11\.10 stacks can use many community cookbooks without modification\.
+ You can use Berkshelf to manage cookbooks\.

  Berkshelf provides a much more flexible way to manage your custom cookbooks and to use community cookbooks in a stack\.
+ Cookbooks must declare dependencies in `metadata.rb`\.

  If your cookbook depends on another cookbook, you must include that dependency in your cookbook's `metadata.rb` file\. For example, if your cookbook includes a recipe with a statement such as `include_recipe anothercookbook::somerecipe`, your cookbook's `metadata.rb` file must include the following line: `depends "anothercookbook"`\.
+ AWS OpsWorks Stacks installs a MySQL client on a stack's instances only if the stack includes a MySQL layer\.
+ AWS OpsWorks Stacks installs a Ganglia client on a stack's instances only if the stack includes a Ganglia layer\.
+ If a deployment runs `bundle install` and the install fails, the deployment also fails\.

**Important**  
Do not reuse built\-in cookbook names for custom or community cookbooks\. Custom cookbooks that have the same name as built\-in cookbooks might fail\. For a complete list of built\-in cookbooks that are available with Chef 11\.10, 11\.4, and 0\.9 stacks, see the [opsworks\-cookbooks repository on GitHub](https://github.com/aws/opsworks-cookbooks)\.  
Cookbooks with non\-ASCII characters that run successfully on Chef 0\.9 and 11\.4 stacks might fail on a Chef 11\.10 stack\. The reason is that Chef 11\.10 stacks use Ruby 2\.0\.0 for Chef runs, which is much stricter about encoding than Ruby 1\.8\.7\. To ensure that such cookbooks run successfully on Chef 11\.10 stacks, each file that uses non\-ASCII characters should have a comment at the top that provides a hint about the encoding\. For example, for UTF\-8 encoding, the comment would be `# encoding: UTF-8`\. For more information on Ruby 2\.0\.0 encoding, see [Encoding](http://www.ruby-doc.org/core-2.0.0/Encoding.html)\.

**Topics**
+ [Cookbook Installation and Precedence](#workingcookbook-chef11-10-override)
+ [Using Chef Search](#workingcookbook-chef11-10-search)
+ [Using Data Bags](#workingcookbook-chef11-10-databag)
+ [Using Berkshelf](#workingcookbook-chef11-10-berkshelf)

## Cookbook Installation and Precedence<a name="workingcookbook-chef11-10-override"></a>

The procedure for installing AWS OpsWorks Stacks cookbooks works somewhat differently for Chef 11\.10 stacks than for earlier Chef versions\. For Chef 11\.10 stacks, after AWS OpsWorks Stacks installs the built\-in, custom, and Berkshelf cookbooks, it merges them to a common directory in the following order:

1. Built\-in cookbooks\.

1. Berkshelf cookbooks, if any\.

1. Custom cookbooks, if any\. 

When AWS OpsWorks Stacks performs this merge, it copies the entire contents of the directories, including recipes\. If there are any duplicates, the following rules apply:
+ The contents of Berkshelf cookbooks take precedence over the built\-in cookbooks\.
+ The contents of custom cookbooks take precedence over the Berkshelf cookbooks\.

To illustrate how this process works, consider the following scenario, where all three cookbook directories include a cookbook named `mycookbook`:
+ Built\-in cookbooks – `mycookbook` includes an attributes file named `someattributes.rb`, a template file named `sometemplate.erb`, and a recipe named `somerecipe.rb`\.
+ Berkshelf cookbooks – `mycookbook` includes `sometemplate.erb` and `somerecipe.rb`\.
+ Custom cookbooks – `mycookbook` includes `somerecipe.rb`\.

The merged cookbook contains the following:
+ `someattributes.rb` from the built\-in cookbook\.
+ `sometemplate.erb` from the Berkshelf cookbook\.
+ `somerecipe.rb` from the custom cookbook\.

**Important**  
You should not customize your Chef 11\.10 stack by copying an entire built\-in cookbook to your repository and then modifying parts of the cookbook\. Doing so overrides the entire built\-in cookbook, including recipes\. If AWS OpsWorks Stacks updates that cookbook, your stack will not get the benefit of those updates unless you manually update your private copy\. For more information on how to customize stacks, see [Customizing AWS OpsWorks Stacks](customizing.md)\.

## Using Chef Search<a name="workingcookbook-chef11-10-search"></a>

You can use the Chef [`search` Method](http://docs.chef.io/dsl_recipe.html#search) in your recipes to query for stack data\. You use the same syntax as you would for Chef server, but AWS OpsWorks Stacks obtains the data from the local node object instead of querying a Chef server\. This data includes:
+ The instance's [stack configuration and deployment attributes](workingstacks-json.md)\.
+ The attributes from the instance's built\-in and custom cookbooks' attributes files\.
+ System data collected by Ohai\.

The stack configuration and deployment attributes contain most of the information that recipes typically obtain through search, including data such as host names and IP addresses for each online instance in the stack\. AWS OpsWorks Stacks updates these attributes for each [lifecycle event](workingcookbook-events.md), which ensures that they accurately reflect the current stack state\. This means that you can often use search\-dependent community recipes in your stack without modification\. The search method still returns the appropriate data; it's just coming from the stack configuration and deployment attributes instead of a server\.

The primary limitation of AWS OpsWorks Stacks search is that handles only the data in the local node object, the stack configuration and deployment attributes in particular\. For that reason, the following types of data might not be available through search:
+ Locally defined attributes on other instances\.

  If a recipe defines an attribute locally, that information is not reported back to the AWS OpsWorks Stacks service, so you cannot access that data from other instances by using search\.
+ Custom `deploy` attributes\.

  You can specify custom JSON when you [deploy an app](workingapps-deploying.md) and the corresponding attributes are installed on the stack's instances for that deployment\. However, if you deploy only to selected instances, the attributes are installed on only those instances\. Queries for those custom JSON attributes will fail on all other instances\. In addition, the custom attributes are included in the stack configuration and deployment JSON only for that particular deployment\. They are accessible only until the next lifecycle event installs a new set of stack configuration and deployment attributes\. Note that if you [specify custom JSON for the stack](workingstacks-json.md), the attributes are installed on every instance for every lifecycle event and are always accessible through search\.
+ Ohai data from other instances\.

  Chef's [Ohai tool](http://docs.chef.io/resource_ohai.html) obtains a variety of system data on an instance and adds it to the node object\. This data is stored locally and not reported back to the AWS OpsWorks Stacks service, so search can't access Ohai data from other instances\. However, some of this data might be included in the stack configuration and deployment attributes\.
+ Offline instances\.

  The stack configuration and deployment attributes contain data only for online instances\.

The following recipe excerpt shows how to get the private IP address of a PHP layer's instance by using search\. 

```
appserver = search(:node, "role:php-app").first
Chef::Log.info("The private IP is '#{appserver[:private_ip]}'")
```

**Note**  
When AWS OpsWorks Stacks adds the stack configuration and deployment attributes to the node object, it actually creates two sets of layer attributes, each with the same data\. One set is in the `layers` namespace, which is how AWS OpsWorks Stacks stores the data\. The other set is in the `role` namespace, which is how Chef server stores the equivalent data\. The purpose of the `role` namespace is to allow search code that was implemented for Chef server to run on an AWS OpsWorks Stacks instance\. If you are writing code specifically for AWS OpsWorks Stacks, you could use either `layers:php-app` or `role:php-app` in the preceding example and `search` would return the same result\.

## Using Data Bags<a name="workingcookbook-chef11-10-databag"></a>

You can use the Chef [`data_bag_item` method](http://docs.chef.io/dsl_recipe.html#data-bag-item) in your recipes to query for information in a data bag\. You use the same syntax as you would for Chef server, but AWS OpsWorks Stacks obtains the data from the instance's stack configuration and deployment attributes\. However, AWS OpsWorks Stacks does not currently support Chef environments, so `node.chef_environment` always returns `_default`\.

You create a data bag by using custom JSON to add one or more attributes to the `[:opsworks][:data_bags]` attribute\. The following example shows the general format for creating a data bag in custom JSON\.

**Note**  
You cannot create a data bag by adding it to your cookbook repository\. You must use custom JSON\.

```
{
  "opsworks": {
    "data_bags": {
      "bag_name1": {
        "item_name1: {
          "key1" : “value1”,
          "key2" : “value2”,
          ...
        }
      },
      "bag_name2": {
        "item_name1": {
          "key1" : “value1”,
          "key2" : “value2”,
          ...
        }
      },
      ...
    }
  }
}
```

You typically [specify custom JSON for the stack](workingstacks-json.md), which installs the custom attributes on every instance for each subsequent lifecycle event\. You can also specify custom JSON when you deploy an app, but those attributes are installed only for that deployment, and might be installed to only a selected set of instances\. For more information, see [Deploying Apps](workingapps-deploying.md)\.

The following custom JSON example creates data bag named `myapp`\. It has one item, `mysql`, with two key\-value pairs\.

```
{ "opsworks": {
    "data_bags": {
      "myapp": {
        "mysql": { 
          "username": "default-user",
          "password": "default-pass"
        }
      }
    }
  }
}
```

To use the data in your recipe, you can call `data_bag_item` and pass it the data bag and value names, as shown in the following excerpt\.

```
mything = data_bag_item("myapp", "mysql")
Chef::Log.info("The username is '#{mything['username']}' ")
```

To modify the data in the data bag, just modify the custom JSON, and it will be installed on the stack's instances for the next lifecycle event\. 

## Using Berkshelf<a name="workingcookbook-chef11-10-berkshelf"></a>

With Chef 0\.9 and Chef 11\.4 stacks, you can install only one custom cookbook repository\. With Chef 11\.10 stacks, you can use [Berkshelf](http://berkshelf.com/) to manage your cookbooks and their dependencies, which allows you to install cookbooks from multiple repositories\. \(For more information, see [Packaging Cookbook Dependencies Locally](best-practices-packaging-cookbooks-locally.md)\.\) In particular, with Berkshelf, you can install AWS OpsWorks Stacks\-compatible community cookbooks directly from their repositories instead of having to copy them to your custom cookbook repository\. The supported Berkshelf versions depend on the operating system\. For more information, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.

To use Berkshelf, you must explicitly enable it, as described in [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)\. Then, include a `Berksfile` file in your cookbook repository's root directory that specifies which cookbooks to install\. 

To specify an external cookbook source in a Berksfile, include a source attribute at the top of the file that specifies the default repository URL\. Berkshelf will look for the cookbooks in the source URLs unless you explicitly specify a repository\. Then include a line for each cookbook that you want to install in the following format: 

```
cookbook 'cookbook_name', ['>= cookbook_version'], [cookbook_options]
```

The fields following `cookbook` specify the particular cookbook\.
+ *cookbook\_name* – \(Required\) Specifies the cookbook's name\.

  If you don't include any other fields, Berkshelf installs the cookbook from the specified source URLs\.
+ *cookbook\_version* – \(Optional\) Specifies the cookbook version or versions\.

  You can use a prefix such as `=` or `>=` to specify a particular version or a range of acceptable versions\. If you don't specify a version, Berkshelf installs the latest one\.
+ *cookbook\_options* – \(Optional\) The final field is a hash containing one or more key\-value pairs that specify options such as the repository location\.

  For example, you can include a `git` key to designate a particular Git repository and a `tag` key to designate a particular repository branch\. Specifying the repository branch is usually the best way to ensure that you install your preferred cookbook\.

**Important**  
Do not declare cookbooks by including a `metadata` line in your Berksfile and declaring the cookbook dependencies in `metadata.rb`\. For this to work correctly, both files must be in the same directory\. With AWS OpsWorks Stacks, the Berksfile must be in the repository's root directory, but `metadata.rb` files must be in their respective cookbook directories\. You should instead explicitly declare external cookbooks in the Berksfile\.

The following is an example of a Berksfile that shows different ways to specify cookbooks For more information on how to create a Berksfile, see [Berkshelf](http://berkshelf.com/)\.

```
source "https://supermarket.chef.io"

cookbook 'apt'
cookbook 'bluepill', '>= 2.3.1'
cookbook 'ark', git: 'git://github.com/opscode-cookbooks/ark.git'
cookbook 'build-essential', '>= 1.4.2', git: 'git://github.com/opscode-cookbooks/build-essential.git', tag: 'v1.4.2'
```

This file installs the following cookbooks:
+ The most recent version of `apt` from the community cookbooks repository\.
+ The most recent version `bluepill` from the community cookbooks, as long as it is version 2\.3\.1 or later\.
+ The most recent version of `ark` from a specified repository\.

  The URL for this example is for a public community cookbook repository on GitHub, but you can install cookbooks from other repositories, including private repositories\. For more information, see [Berkshelf](http://berkshelf.com/)\.
+ The `build-essential` cookbook from the v1\.4\.2 branch of the specified repository\.

A custom cookbook repository can contain custom cookbooks in addition to a Berksfile\. In that case, AWS OpsWorks Stacks installs both sets of cookbooks, which means that an instance can have as many as three cookbook repositories\. 
+ The built\-in cookbooks are installed to `/opt/aws/opsworks/current/cookbooks`\.
+ If your custom cookbook repository contains cookbooks, they are installed to `/opt/aws/opsworks/current/site-cookbooks`\.
+ If you have enabled Berkshelf and your custom cookbook repository contains a Berksfile, the specified cookbooks are installed to `/opt/aws/opsworks/current/berkshelf-cookbooks`\.

The built\-in cookbooks and your custom cookbooks are installed on each instance during setup and are not subsequently updated unless you manually run the [**Update Custom Cookbooks** stack command](workingstacks-commands.md)\. AWS OpsWorks Stacks runs `berks install` for every Chef run, so your Berkshelf cookbooks are updated for each [lifecycle event](workingcookbook-events.md), according to the following rules: 
+ If you have a new cookbook version in the repository, this operation updates the cookbook from the repository\.
+ Otherwise, this operation updates the Berkshelf cookbooks from a local cache\.

**Note**  
The operation overwrites the Berkshelf cookbooks, so if you have modified the local copies of any cookbooks, the changes will be overwritten\. For more information, see [Berkshelf](http://berkshelf.com/)

You can also update your Berkshelf cookbooks by running the **Update Custom Cookbooks** stack command, which updates both the Berkshelf cookbooks and your custom cookbooks\.