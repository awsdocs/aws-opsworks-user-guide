# Installing and Configuring Packages on Linux Instances<a name="workingcookbook-extend-package"></a>

The built\-in layers support only certain packages\. For more information, see [Layers](workinglayers.md)\. You can install other packages, such as a Redis server, by implementing custom recipes to handle the associated setup, configuration, and deployment tasks\. In some cases, the best approach is to extend a built\-in layer to have it install the package on its instances alongside the layer's standard packages\. For example, if you have a stack that supports a PHP application, and you would like to include a Redis server, you could extend the PHP App Server layer to install and configure a Redis server on the layer's instances in addition to a PHP application server\.

A package installation recipe typically needs to perform tasks like these:
+ Create one or more directories and set their modes\.
+ Create a configuration file from a template\.
+ Run the installer to install the package on the instance\.
+ Start one or more services\.

For an example of how to install a Tomcat server, see [Creating a Custom Tomcat Server Layer](create-custom.md)\. The topic describes how to set up a custom Redis layer, but you could use much the same code to install and configure Redis on a built\-in layer\. For examples of how to install other packages, see the built\-in cookbooks, at [https://github\.com/aws/opsworks\-cookbooks](https://github.com/aws/opsworks-cookbooks)\.