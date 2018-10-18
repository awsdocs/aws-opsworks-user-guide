# Cookbook Components<a name="workingcookbook-installingcustom-components"></a>

A cookbook typically includes the following basic components:
+ **Attribute** files contain a set of attributes that represent values to be used by the recipes and templates\.
+ **Template** files are templates that recipes use to create other files, such as configuration files\.

  Template files typically let you modify the configuration file by overriding attributes—which can be done without touching the cookbook—instead of rewriting a configuration file\. The standard practice is that whenever you expect to change a configuration file on an instance even slightly, you should use a template file\. 
+ **Recipe** files are Ruby applications that define everything that is required to configure a system, including creating and configuring folders, installing and configuring packages, starting services, and so on\.

Cookbooks don't have to have all three components\. The simpler approaches to customization require only attribute or template files\. In addition, cookbooks can optionally include other file types, such as definitions or specs\. 

This section describes the three standard cookbook components\. For more information, especially about how to implement recipes, see [Opscode](http://www.opscode.com/chef/)\.

**Topics**
+ [Attributes](workingcookbook-installingcustom-components-attributes.md)
+ [Templates](workingcookbook-installingcustom-components-templates.md)
+ [Recipes](workingcookbook-installingcustom-components-recipes.md)