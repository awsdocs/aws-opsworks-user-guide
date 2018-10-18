# Customizing AWS OpsWorks Stacks<a name="customizing"></a>

AWS OpsWorks Stacks built\-in layers provide standard functionality that is sufficient for many purposes\. However, you might encounter one or more of the following:
+ A built\-in layer's standard configuration is adequate but not ideal; you would like to optimize it for your particular requirements\.

  For example, you might want to tune a Static Web Server layer's Nginx server configuration by specifying your own values for settings such as the maximum number of worker processes or the `keepalivetimeout` value\.
+ A built\-in layer's functionality is fine, but you want to extend it by installing additional packages or running some custom installation scripts\.

  For example, you might want to extend a PHP App Server layer by also installing a Redis server\.
+ You have requirements that aren't handled by any of the built\-in layers\.

  For example, AWS OpsWorks Stacks does not include built\-in layers for some popular database servers\. You can create a custom layer that installs those servers on the layer's instances\.
+ You are running a Windows stack, which support only custom layers\.

AWS OpsWorks Stacks provides a variety of ways to customize layers to meet your specific requirements\. The following examples are listed in order of increasing complexity and power:

**Note**  
Some of these approaches work only for Linux stacks\. See the following topics for details\.
+ Use custom JSON to override default AWS OpsWorks Stacks settings\.
+ Implement a custom Chef cookbook with an attributes file that overrides the default AWS OpsWorks Stacks settings\.
+ Implement a custom Chef cookbook with a template that overrides or extends a default AWS OpsWorks Stacks template\.
+ Implement a custom Chef cookbook with a simple recipe that runs a shell script\.
+ Implement a custom Chef cookbook with recipes that perform tasks such as creating and configuring directories, installing packages, creating configuration files, deploying apps, and so on\. 

You can also override recipes, depending on the stack's Chef version and operating system\.
+ With Chef 0\.9 and 11\.4 stacks, you cannot override a built\-in recipe by implementing a custom recipe with the same cookbook and recipe name\.

  For each lifecycle event, AWS OpsWorks Stacks always runs the built\-in recipes first, followed by any custom recipes\. Because these Chef versions do not run a recipe with the same cookbook and recipe name twice, the built\-in recipe takes precedence and the custom recipe is not executed\.
+ You can override built\-in recipes on Chef 11\.10 stacks\.

  For more information, see [Cookbook Installation and Precedence](workingcookbook-chef11-10.md#workingcookbook-chef11-10-override)\.
+ You cannot override built\-in recipes on Windows stacks\.

  The way that AWS OpsWorks Stacks handles Chef runs for Windows stacks does not allow built\-in recipes to be overridden\.

**Note**  
Because many of the techniques use custom cookbooks, you should first read [Cookbooks and Recipes](workingcookbook.md) if you are not already familiar with cookbook implementation\. [Cookbook Basics](cookbooks-101-basics.md) provides a detailed tutorial introduction to implementing custom cookbooks, and [Implementing Cookbooks for AWS OpsWorks Stacks](cookbooks-101-opsworks.md) covers some of the details about how to implement cookbooks for AWS OpsWorks Stacks instances\.

**Topics**
+ [Customizing AWS OpsWorks Stacks Configuration by Overriding Attributes](workingcookbook-attributes.md)
+ [Extending AWS OpsWorks Stacks Configuration Files Using Custom Templates](workingcookbook-template-override.md)
+ [Extending a Layer](workingcookbook-extend.md)
+ [Creating a Custom Tomcat Server Layer](create-custom.md)
+ [Stack Configuration and Deployment Attributes](workingcookbook-json.md)