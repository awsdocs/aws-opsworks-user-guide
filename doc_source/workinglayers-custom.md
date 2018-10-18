# Custom AWS OpsWorks Stacks Layers<a name="workinglayers-custom"></a>

A custom layer has only a minimal set of recipes\. You then add appropriate functionality to the layer by implementing [custom recipes](workingcookbook.md) and assigning them to the layer's [lifecycle events](workingcookbook-events.md)\.

The custom layer has the following configuration settings\.

**Note**  
AWS OpsWorks Stacks automatically installs Ruby on the layer's instances\. If you want to run Ruby code on the instance but don't want to use the default Ruby version, you can use custom JSON or a custom attributes file to specify your preferred version\. For more information, see [Ruby Versions](workingcookbook-ruby.md)\.

The basic procedure for creating a custom layer has the following steps:

1. Implement a [cookbook](workingcookbook.md) that contains the recipes and associated files required to install and configure packages, handle configuration changes, deploy apps, and so on\.

   Depending on your requirements, you might also need recipes to handle undeployment and shutdown tasks\. For more information, see [Cookbooks and Recipes](workingcookbook.md)\.

1. Create a custom layer\.

1. Assign your recipes to the appropriate [lifecycle events](workingcookbook-events.md)\.

You then add instances to the layer, start them, and deploy apps to those instances\.

**Important**  
To deploy apps to a custom layer's instances, you must implement recipes to handle the deploy operation and assign them to the layer's Deploy event\.