# Creating a Custom Tomcat Server Layer<a name="create-custom"></a>

**Note**  
This topic describes how to implement a custom layer for a Linux stack\. However, the basic principles and some of the code can also be adapted to implement custom layers for Windows stacks, especially those in the section on app deployment\.

The simplest way to use nonstandard packages on AWS OpsWorks Stacks instances is to [extend an existing layer](workingcookbook-extend-package.md)\. However, this approach installs and runs both the standard and nonstandard packages on the layer's instances, which is not always desirable\. A somewhat more demanding but more powerful approach is to implement a custom layer, which gives you almost complete control over the layer's instances, including the following: 
+ Which packages are installed
+ How each package is configured
+ How to deploy apps from a repository to the instance

Whether using the console or API, you create and manage a custom layer much like any other layer, as described in [Custom Layers](workinglayers-custom.md)\. However, a custom layer's built\-in recipes perform only some very basic tasks, such as installing a Ganglia client to report metrics to a Ganglia master\. To make a custom layer's instances more than minimally functional, you must implement one or more custom cookbooks with Chef recipes and related files to handle the tasks of installing and configuring packages, deploying apps, and so on\. You don't necessarily have to implement everything from scratch, though\. For example, if you store applications in one of the standard repositories, you can use the built\-in deploy recipes to handle much of the work of installing the applications on the layer's instances\.

**Note**  
If you are new to Chef, you should first read [Cookbooks 101](cookbooks-101.md), which is a tutorial that introduces the basics of how to implement cookbooks to perform a variety of common tasks\. 

The following walkthrough describes how to implement a custom layer that supports a Tomcat application server\. The layer is based on a custom cookbook named Tomcat, which includes recipes to handle package installation, deployment, and so on\. The walkthrough includes excerpts from the Tomcat cookbook\. You can download the complete cookbook from its [GitHub repository](https://github.com/amazonwebservices/opsworks-example-cookbooks/tree/master/tomcat)\. If you are not familiar with [Opscode Chef](http://www.opscode.com/chef/), you should first read [Cookbooks and Recipes](workingcookbook.md)\.

**Note**  
AWS OpsWorks Stacks includes a full\-featured [Java App Server layer](layers-java.md) for production use\. The purpose of the Tomcat cookbook is to show how to implement custom layers, so it supports only a limited version of Tomcat that does not include features such as SSL\. For an example of a full featured implementation, see the built\-in [opsworks\_java](https://github.com/aws/opsworks-cookbooks/tree/release-chef-11.10/opsworks_java) cookbook \.

The Tomcat cookbook supports a custom layer whose instances have the following characteristics:
+ They support a Tomcat Java application server with an Apache front end\.
+ Tomcat is configured to allow applications to use a JDBC `DataSource` object to connect to a separate MySQL instance, which serves as a back end data store\.

The cookbook for this project involves several key components:
+ [Attributes file](create-custom-attributes.md) contains configuration settings that are used by the various recipes\.
+ [Setup recipes](create-custom-setup.md) are assigned to the layer's Setup [lifecycle event](workingcookbook-events.md)\. They run after an instance has booted and perform tasks such as installing packages and creating configuration files\.
+ [Configure recipes](create-custom-configure.md) are assigned to the layer's Configure lifecycle event\. They run after the stack's configuration changes—primarily when instances come online or go offline—and handle any required configuration changes\.
+ [Deploy recipes](create-custom-deploy.md) are assigned to the layer's Deploy lifecycle event\. They run after the Setup recipes and when you manually deploy an app to install the code and related files on a layer's instances and handle related tasks, such as restarting services\.

The final section, [Create a Stack and Run an Application](create-custom-stack.md), describes how to create a stack that includes a custom layer based on the Tomcat cookbook and how to deploy and run a simple JSP application that displays data from a MySQL database running on an instance that belongs to a separate MySQL layer\.

**Note**  
The Tomcat cookbook recipes depend on some AWS OpsWorks Stacks built\-in recipes\. To make each recipe's origin clear, this topic identifies recipes using the Chef *cookbookname*::*recipename* convention\.

**Topics**
+ [Attributes File](create-custom-attributes.md)
+ [Setup Recipes](create-custom-setup.md)
+ [Configure Recipes](create-custom-configure.md)
+ [Deploy Recipes](create-custom-deploy.md)
+ [Create a Stack and Run an Application](create-custom-stack.md)