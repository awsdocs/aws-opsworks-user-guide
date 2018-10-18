# Cookbooks and Recipes<a name="workingcookbook"></a>

AWS OpsWorks Stacks uses Chef cookbooks to handle tasks such as installing and configuring packages and deploying apps\. This section describes how to use cookbooks with AWS OpsWorks Stacks\. For more information, see [Chef](http://www.opscode.com/)\.

**Note**  
AWS OpsWorks Stacks currently supports Chef versions 12, 11\.10\.4, 11\.4\.4, and 0\.9\.15\.5\. However, Chef 0\.9\.15\.5 is deprecated and we do not recommend that you use it for new stacks\. For convenience, they are usually referred to by just their major and minor version numbers\. Stacks running Chef 0\.9 or 11\.4 use [Chef Solo](https://docs.chef.io/chef_solo.html) and stacks running Chef 12 or 11\.10 use [Chef Client](http://www.getchef.com/blog/2013/10/31/chef-client-z-from-zero-to-chef-in-8-5-seconds/) in local mode\. For Linux stacks, you can use the Configuration Manager to specify which Chef version to use when you [create a stack](workingstacks-creating.md)\. Windows stacks must use Chef 12\.2\. For more information, including guidelines for migrating stacks to more recent Chef versions, see [Chef Versions](workingcookbook-chef11.md)\.

**Topics**
+ [Cookbook Repositories](workingcookbook-installingcustom-repo.md)
+ [Chef Versions](workingcookbook-chef11.md)
+ [Ruby Versions](workingcookbook-ruby.md)
+ [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)
+ [Updating Custom Cookbooks](workingcookbook-installingcustom-enable-update.md)
+ [Executing Recipes](workingcookbook-executing.md)