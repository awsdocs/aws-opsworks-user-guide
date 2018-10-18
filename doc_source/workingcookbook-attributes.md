# Customizing AWS OpsWorks Stacks Configuration by Overriding Attributes<a name="workingcookbook-attributes"></a>

**Note**  
For Windows stacks and Chef 12 Linux stacks, AWS OpsWorks Stacks uses separate Chef runs for built\-in recipes and custom recipes\. This means that you cannot use the techniques discussed in this section to override built\-in attributes for Windows stacks and Chef 12 Linux stacks\.

Recipes and templates depend on a variety of Chef attributes for instance or stack\-specific information such as layer configurations or application server settings\. These attributes have several sources:
+ **Custom JSON**–You can optionally specify custom JSON attributes when you create, update, or clone a stack, or when you deploy an app\.
+ **Stack configuration attributes**–AWS OpsWorks Stacks defines these attributes to hold stack configuration information, including the information that you specify through the console settings\. 
+ **Deployment attributes**–AWS OpsWorks defines deployment\-related attributes for Deploy events\.
+ **Cookbook attributes**– Built\-in and custom Cookbooks usually include one or more [attribute files](workingcookbook-installingcustom-components-attributes.md), which contain attributes that represent cookbook\-specific values such as application server configuration settings\. 
+ **Chef**–Chef's [Ohai tool](http://docs.chef.io/resource_ohai.html) defines attributes that represent a wide variety of system configuration settings, such as CPU type and installed memory\.

For a complete list of stack configuration and deployment attributes and built\-in cookbook attributes , see [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md) and [Built\-in Cookbook Attributes](attributes-recipes.md)\. For more information about Ohai attributes, see [Ohai](https://docs.chef.io/ohai.html)\.

When a [lifecycle event](workingcookbook-events.md) such as Deploy or Configure occurs, or you run a [stack command](workingstacks-commands.md) such as `execute_recipes` or `update_packages`, AWS OpsWorks Stacks does the following:
+ Sends a corresponding command to the agent on each affected instance\.

  The agent runs the appropriate recipes\. For example, for a Deploy event, the agent runs the built\-in Deploy recipes, followed by any custom Deploy recipes\.
+ Merges any custom JSON and deployment attributes with the stack configuration attributes and installs them on the instances\.

The attributes from custom JSON, stack configuration and deployment attributes, cookbook attributes, and Ohai attributes are merged into a *node object*, which supplies attribute values to recipes\. An instance is essentially stateless as far as stack configuration attributes are concerned, including any custom JSON\. When you run a deployment or stack command, the associated recipes use the stack configuration attributes that were downloaded with the command\.

**Topics**
+ [Attribute Precedence](workingcookbook-attributes-precedence.md)
+ [Overriding Attributes With Custom JSON](workingcookbook-json-override.md)
+ [Overriding AWS OpsWorks Stacks Attributes Using Custom Cookbook Attributes](workingcookbook-cookbook-attributes.md)