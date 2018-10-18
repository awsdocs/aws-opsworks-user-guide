# AWS OpsWorks Stacks Support for Chef 12 Linux<a name="chef-12-linux"></a>

This section provides a brief overview of AWS OpsWorks Stacks for Chef 12 Linux\. For information about Chef 12 on Windows, see [Getting Started: Windows](gettingstarted-windows.md)\. For information about previous Chef versions on Linux, see [Chef 11\.10 and Earlier Versions for Linux](chef-11-linux.md)\.

## Overview<a name="chef-12-linux-overview"></a>

 AWS OpsWorks Stacks supports Chef 12, the latest version of Chef, for Linux stacks\. For more information, see [Learn Chef](https://docs.chef.io/)\. 

 AWS OpsWorks Stacks continues to support Chef 11\.10 for Linux stacks\. However, if you are an advanced Chef user who would like to benefit from the large selection of community cookbooks or write your own custom cookbooks, we recommend that you use Chef 12\. Chef 12 stacks provide the following advantages over Chef 11\.10 and earlier stacks for Linux: 
+ **Two separate Chef runs** \- When a command is executed on an instance, the AWS OpsWorks Stacks agent now executes two isolated Chef runs: one run for tasks that integrate the instance with other AWS services like AWS Identity and Access Management \(IAM\), and one run for your custom cookbooks\. The first Chef run installs the AWS OpsWorks Stacks agent on the instance and performs system tasks such as user setup and management, volume setup and configuration, configuration of CloudWatch metrics, and so on\. The second run is dedicated exclusively to running your custom recipes for [AWS OpsWorks Stacks Lifecycle Events](workingcookbook-events.md)\. This second run lets you use your own Chef cookbooks or community cookbooks\. 
+ **Resolution of namespace conflicts** \- Prior to Chef 12, AWS OpsWorks Stacks performed system tasks and ran built\-in and custom recipes in a shared environment\. This resulted in namespace conflicts and lack of clarity about which recipes AWS OpsWorks Stacks had run\. Unwanted default configurations had to be manually overwritten, a time consuming and error\-prone task\. In Chef 12 for Linux, AWS OpsWorks Stacks no longer supports built\-in Chef cookbooks for application server environments like PHP, Node\.js, or Rails\. By eliminating built\-in recipes, AWS OpsWorks Stacks eliminates the issue of naming collisions between built\-in and custom recipes\.
+ **Strong support for Chef community cookbooks** – AWS OpsWorks Stacks Chef 12 Linux offers greater compatibility and support for community cookbooks from the Chef supermarket\. You can now use community cookbooks that are superior to the built\-in cookbooks that AWS OpsWorks Stacks previously provided—cookbooks that are designed for use with the latest application server environments and frameworks\. You can run most of these cookbooks without modification on Chef 12 for Linux\. For more information, go to [Chef Supermarket](https://docs.chef.io/supermarket.html) on the [Learn Chef](https://docs.chef.io/) website, the [Chef Supermarket](https://supermarket.chef.io/) website, and the [Chef Cookbooks](https://github.com/chef-cookbooks) repository on [GitHub](https://github.com/)\. 
+ **Timely Chef 12 updates** \- AWS OpsWorks Stacks will update its Chef environment to the latest Chef 12 version shortly after each Chef release\. With Chef 12, minor Chef updates and new AWS OpsWorks Stacks agent releases will coincide\. This lets you test new Chef releases directly, and enables your Chef recipes and applications to take advantage of the latest Chef features\. 

For more information about supported Chef versions prior to Chef 12, see [Chef 11\.10 and Earlier Versions for Linux](chef-11-linux.md)\.

## Moving to Chef 12<a name="chef-12-linux-moving-to"></a>

Key AWS OpsWorks Stacks changes for Chef 12 Linux, as compared to support for previous Chef versions 11\.10, 11\.4, and 0\.9, are as follows: 
+ Built\-in layers are no longer provided or supported for Chef 12 for Linux stacks\. Because only your custom recipes are executed, removing this support gives total transparency into how the instance is set up and makes custom cookbooks much easier to write and maintain\. For example, it's no longer necessary to overwrite attributes of built\-in AWS OpsWorks Stacks recipes\. Removal of built\-in layers also enables AWS OpsWorks Stacks to better support cookbooks that are developed and maintained by the Chef community, so that you can take full advantage of them\. The built\-in layer types no longer available in Chef 12 for Linux are: [AWS Flow \(Ruby\)](https://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-awsflow.html), [Ganglia](https://docs.aws.amazon.com/opsworks/latest/userguide/layers-other-ganglia.html), [HAProxy](https://docs.aws.amazon.com/opsworks/latest/userguide/layers-haproxy.html), [Java App Server](https://docs.aws.amazon.com/opsworks/latest/userguide/layers-java.html), [Memcached](https://docs.aws.amazon.com/opsworks/latest/userguide/layers-other-memcached.html), [MySQL](https://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-db-mysql.html), [Node\.js App Server](https://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-node.html), [PHP App Server](https://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-php.html), [Rails App Server](https://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-rails.html), and [Static Web Server](https://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-static.html)\. 
  + Because AWS OpsWorks Stacks is running recipes that you provide, there is no longer a need to override built\-in AWS OpsWorks Stacks attributes by running custom cookbooks\. To override attributes in your own or community recipes, follow instructions and examples in [About Attributes](https://docs.chef.io/attributes.html) in the Chef 12 documentation\.
+ AWS OpsWorks Stacks continues to provide support for the following layers for Chef 12 Linux stacks: 
  + [Custom Layers](workinglayers-custom.md)
  + [Amazon RDS Service Layer](workinglayers-db-rds.md)
  + [ECS Cluster Layers](workinglayers-ecscluster.md)
+ Stack configuration and data bags for Chef 12 Linux have changed to look very similar to their counterparts for Chef 12\.2 Windows\. This makes it easier to query for, analyze, and troubleshoot these data bags, especially if you work with stacks with different operating system types\. Note that AWS OpsWorks Stacks does not support encrypted data bags\. To store sensitive data in encrypted form, such as passwords or certificates, we recommend storing it in a private S3 bucket\. You can then create a custom recipe that uses the [Amazon SDK for Ruby](http://aws.amazon.com/documentation/sdk-for-ruby/) to retrieve the data\. For an example, see [Using the SDK for Ruby](cookbooks-101-opsworks-s3.md)\.For more information, see [AWS OpsWorks Stacks Data Bag Reference](data-bags.md)\.
+ In Chef 12 Linux, Berkshelf is no longer installed on stack instances\. Instead, we recommend that you use Berkshelf on a local development machine to package your cookbook dependencies locally\. Then upload your package, with the dependencies included, to Amazon Simple Storage Service\. Finally, modify your Chef 12 Linux stack to use the uploaded package as a cookbook source\. For more information, see [Packaging Cookbook Dependencies Locally](best-practices-packaging-cookbooks-locally.md)\.
+ RAID configurations for EBS volumes are no longer supported\. For increased performance, you can use [provisioned IOPS for Amazon Elastic Block Store \(Amazon EBS\)](https://aws.amazon.com/about-aws/whats-new/2012/07/31/announcing-provisioned-iops-for-amazon-ebs/)\.
+ autofs is no longer supported\.
+ Subversion repositories are no longer supported\.
+ Per\-layer OS package installations must now be done with custom recipes\. For more information, see [Per\-layer Package Installations](per-layer-os-package-install.md)\.

## Supported Operating Systems<a name="chef-12-linux-supported-oses"></a>

Chef 12 supports the same Linux operating systems as previous versions of Chef\. For a list of Linux operating system types and versions that Chef 12 Linux stacks can use, see [Linux Operating Systems](workinginstances-os-linux.md)\.

## Supported Instance Types<a name="chef-12-linux-supported-instance-types"></a>

AWS OpsWorks Stacks supports all instance types for Chef 12 Linux stacks except specialized instance types like high performance computing \(HPC\) cluster compute, cluster GPU, and high memory cluster instance types\.

## More Information<a name="chef-12-linux-more-info"></a>

 To learn more about how to work with Chef 12 for Linux stacks, see the following:
+ [Getting Started: Sample](gettingstarted-intro.md)

  Introduces you to AWS OpsWorks Stacks by guiding you through a brief hands\-on exercise with the AWS OpsWorks Stacks console to create a Node\.js application environment\.
+  [Getting Started: Linux](gettingstarted-linux.md)

  Introduces you to AWS OpsWorks Stacks and Chef 12 Linux by guiding you through a hands\-on exercise with the AWS OpsWorks Stacks console to create a basic Chef 12 Linux stack that contains a simple layer with a Node\.js app that serves traffic\. 
+ [Custom Layers](workinglayers-custom.md)

  Provides guidance for adding a layer that contains cookbooks and recipes to a Chef 12 Linux stack\. You can use readily available cookbooks and recipes that the Chef community provides, or you can create your own\. 
+ [Moving to Data Bags](attributes-to-data-bags.md)

  Compares and contrasts instance JSON that is used by Linux stacks running Chef 11 and earlier versions with Chef 12\. Also provides pointers to reference documentation for the Chef 12 instance JSON format\.