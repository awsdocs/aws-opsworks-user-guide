# Overriding AWS OpsWorks Stacks Attributes Using Custom Cookbook Attributes<a name="workingcookbook-cookbook-attributes"></a>

**Note**  
For Windows stacks, AWS OpsWorks Stacks uses separate Chef runs for built\-in recipes and custom recipes\. This means that you cannot use the techniques discussed in this section to override built\-in attributes for Windows stacks\.

Custom JSON is a convenient way to override AWS OpsWorks Stacks stack configuration and built\-in cookbook attributes, but it has some limitations\. In particular, you must enter custom JSON manually for each use, so you have no robust way to manage the definitions\. A better approach is often to use custom cookbook attribute files to override built\-in attributes\. Doing so allows you to place the definitions under source control\.

The procedure for using custom attribute files to override AWS OpsWorks Stacks definitions is straightforward\.

**To override AWS OpsWorks Stacks attribute definitions**

1. Set up a cookbook repository, as described in [Cookbooks and Recipes](workingcookbook.md)\.

1. Create a cookbook with the same name as the built\-in cookbook that contains the attributes that you want to override\. For example, to override the Apache attributes, the cookbook should be named apache2\. 

1. Add an `attributes` folder to the cookbook and add a file to that folder named `customize.rb`\. 

1. Add an attribute definition to the file for each of the built\-in cookbook's attributes that you want to override, set to your preferred value\. The attribute must be a `normal` type or higher and have exactly the same node name as the corresponding AWS OpsWorks Stacks attribute\. For a detailed list of AWS OpsWorks Stacks attributes, including node names, see [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md) and [Built\-in Cookbook Attributes](attributes-recipes.md)\. For more information on attributes and attributes files, see [About Attribute Files](http://docs.chef.io/attributes.html)\.
**Important**  
Your attributes must be `normal` type to override AWS OpsWorks Stacks attributes; `default` types do not have precedence\. For example, if your `customize.rb` file contains a `default[:apache][:keepalivetimeout] = 5` attribute definition, the corresponding attribute in the built\-in `apache.rb` attributes file is evaluated first, and takes precedence\. For more information, see [Overriding Attributes](workingcookbook-attributes.md)\.

1. Repeat Steps 2 – 4 for each built\-in cookbook with attributes that you want to override\.

1. Enable custom cookbooks for your stack and provide the information required for AWS OpsWorks Stacks to download your cookbooks to the stack's instances\. For more information, see [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)\.

**Note**  
For a complete walkthrough of this procedure, see [Overriding Built\-In Attributes](cookbooks-101-opsworks-attributes.md)\.

The node object used by subsequent lifecycle events, deploy commands, and stack commands will now contain your attribute definitions instead of the AWS OpsWorks Stacks values\. 

For example, to override the built\-in Apache `keepalivetimeout` and `logrotate schedule` settings discussed in [How to Specify Custom JSON](workingcookbook-json-override.md#workingcookbook-json-override-specify), add an `apache2` cookbook to your repository and add a `customize.rb` file to the cookbook's `attributes` folder with the following contents\.

```
normal[:apache][:keepalivetimeout] = 5
normal[:apache][:logrotate][:schedule] = 'weekly'
```

**Important**  
You should not override AWS OpsWorks Stacks attributes by modifying a copy of the associated built\-in attributes file\. If, for example, you copy `apache.rb` to your `apache2/attributes` folder and modify some of its settings, you essentially override every attribute in the built\-in file\. Recipes will use the attribute definitions from your copy and ignore the built\-in file\. If AWS OpsWorks Stacks later modifies the built\-in attributes file, recipes will not have access to the changes unless you manually update your copy\.   
To avoid this situation, all built\-in cookbooks contain an empty `customize.rb` attributes file, which is required in all modules through an `include_attribute` directive\. By overriding attributes in your copy of `customize.rb`, you affect only those specific attributes\. Recipes will obtain any other attribute values from the built\-in attributes files, and automatically get the current values of any attributes that you have not overridden\.  
This approach helps you to keep the number of attributes in your cookbook repository small, which reduces your maintenance overhead and makes future upgrades easier to manage\.