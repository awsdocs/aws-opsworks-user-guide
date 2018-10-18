# AWS OpsWorks Stacks<a name="welcome_classic"></a>

Cloud\-based computing usually involves groups of AWS resources, such as Amazon EC2 instances and Amazon Relational Database Service \(RDS\) instances, which must be created and managed collectively\. For example, a web application typically requires application servers, database servers, load balancers, and so on\. This group of instances is typically called a *stack*; a simple application server stack might look something like the following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch.png)

In addition to creating the instances and installing the necessary packages, you typically need a way to distribute applications to the application servers, monitor the stack's performance, manage security and permissions, and so on\.

AWS OpsWorks Stacks provides a simple and flexible way to create and manage stacks and applications\.

Here's how a basic application server stack might look with AWS OpsWorks Stacks\. It consists of a group of application servers running behind an Elastic Load Balancing load balancer, with a backend Amazon RDS database server\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch_4.png)

Although relatively simple, this stack shows all the key AWS OpsWorks Stacks features\. Here's how it's put together\.

**Topics**
+ [Stacks](#welcome-classic-stacks)
+ [Layers](#welcome-classic-layers)
+ [Recipes and LifeCycle Events](#welcome-classic-lifecycle)
+ [Instances](#welcome-classic-instances)
+ [Apps](#welcome-classic-apps)
+ [Customizing your Stack](#welcome-classic-customizing)
+ [Resource Management](#welcome-classic-resources)
+ [Security and Permissions](#welcome-classic-security)
+ [Monitoring and Logging](#welcome-classic-monitoring)
+ [CLI, SDK, and AWS CloudFormation Templates](#welcome-classic-sdk)
+ [Getting Started with AWS OpsWorks Stacks](gettingstarted_intro.md)
+ [AWS OpsWorks Stacks Best Practices](best-practices.md)
+ [Stacks](workingstacks.md)
+ [Layers](workinglayers.md)
+ [Instances](workinginstances.md)
+ [Apps](workingapps.md)
+ [Cookbooks and Recipes](workingcookbook.md)
+ [Resource Management](resources.md)
+ [Tags](tagging.md)
+ [Monitoring](monitoring.md)
+ [Security and Permissions](workingsecurity.md)
+ [AWS OpsWorks Stacks Support for Chef 12 Linux](chef-12-linux.md)
+ [Support for Previous Chef Versions in AWS OpsWorks Stacks](previous-chef-versions.md)
+ [Using AWS OpsWorks Stacks with Other AWS Services](other-services.md)
+ [Using the AWS OpsWorks Stacks CLI](cli-examples.md)
+ [Debugging and Troubleshooting Guide](troubleshoot.md)
+ [AWS OpsWorks Stacks Agent CLI](agent.md)
+ [AWS OpsWorks Stacks Data Bag Reference](data-bags.md)
+ [OpsWorks Agent Changes](agentchanges.md)

## Stacks<a name="welcome-classic-stacks"></a>

The *stack* is the core AWS OpsWorks Stacks component\. It is basically a container for AWS resources—Amazon EC2 instances, Amazon RDS database instances, and so on—that have a common purpose and should be logically managed together\. The stack helps you manage these resources as a group and also defines some default configuration settings, such as the instances' operating system and AWS region\. If you want to isolate some stack components from direct user interaction, you can run the stack in a VPC\.

## Layers<a name="welcome-classic-layers"></a>

You define the stack's constituents by adding one or more *layers*\. A layer represents a set of Amazon EC2 instances that serve a particular purpose, such as serving applications or hosting a database server\.

You can customize or extend layers by modifying packages' default configurations, adding Chef recipes to perform tasks such as installing additional packages, and more\. 

For all stacks, AWS OpsWorks Stacks includes *service layers*, which represent the following AWS services\.
+ Amazon Relational Database Service 
+ Elastic Load Balancing
+ Amazon Elastic Container Service

Layers give you complete control over which packages are installed, how they are configured, how applications are deployed, and more\.

## Recipes and LifeCycle Events<a name="welcome-classic-lifecycle"></a>

Layers depend on [Chef recipes](http://docs.chef.io/recipes.html) to handle tasks such as installing packages on instances, deploying apps, running scripts, and so on\. One of the key AWS OpsWorks Stacks features is a set of *lifecycle events*—Setup, Configure, Deploy, Undeploy, and Shutdown—which automatically run a specified set of recipes at the appropriate time on each instance\.

Each layer can have a set of recipes assigned to each lifecycle event, which handle a variety of tasks for that event and layer\. For example, after an instance that belongs to a web server layer finishes booting, AWS OpsWorks Stacks does the following\. 

1. Runs the layer's Setup recipes, which could perform tasks such as installing and configuring a web server\.

1. Runs the layer's Deploy recipes, which deploy the layer's applications from a repository to the instance and perform related tasks, such as restarting the service\.

1. Runs the Configure recipes on every instance in the stack so each instance can adjust its configuration as needed to accommodate the new instance\.

   For example, on an instance running a load balancer, a Configure recipe could modify the load balancer's configuration to include the new instance\.

If an instance belongs to multiple layers, AWS OpsWorks Stacks runs the recipes for each layer so you can, for example, have an instance that supports a PHP application server and a MySQL database server\.

If you have implemented recipes, you can assign each recipe to the appropriate layer and event and AWS OpsWorks Stacks automatically runs them for you at the appropriate time\. You can also run recipes manually, at any time\.

## Instances<a name="welcome-classic-instances"></a>

An *instance* represents a single computing resource, such as an Amazon EC2 instance\. It defines the resource's basic configuration, such as operating system and size\. Other configuration settings, such as Elastic IP addresses or Amazon EBS volumes, are defined by the instance's layers\. The layer's recipes complete the configuration by performing tasks such as installing and configuring packages and deploying apps\.

You can use AWS OpsWorks Stacks to create instances and add them to a layer\. When you start the instance, AWS OpsWorks Stacks launches an Amazon EC2 instance using the configuration settings specified by the instance and its layer\. After the Amazon EC2 instance has finished booting, AWS OpsWorks Stacks installs an agent that handles communication between the instance and the service and runs the appropriate recipes in response to lifecycle events\. 

AWS OpsWorks Stacks supports the following instance types, which are characterized by how they are started and stopped\.
+ **24/7 instances **are started manually and run until you stop them\.
+ **Time\-based instances** are run by AWS OpsWorks Stacks on a specified daily and weekly schedule\.

  They allow your stack to automatically adjust the number of instances to accommodate predictable usage patterns\.
+ **Load\-based instances** are automatically started and stopped by AWS OpsWorks Stacks, based on specified load metrics, such as CPU utilization\.

  They allow your stack to automatically adjust the number of instances to accommodate variations in incoming traffic\. Load\-based instances are available only for Linux\-based stacks\.

AWS OpsWorks Stacks supports instance autohealing\. If an agent stops communicating with the service, AWS OpsWorks Stacks automatically stops and restarts the instance\.

You can also incorporate Linux\-based computing resources into a stack that was created outside of AWS OpsWorks Stacks\.
+ Amazon EC2 instances that you created directly by using the Amazon EC2 console, CLI, or API\.
+ *On\-premises* instances running on your own hardware, including instances running in virtual machines\.

After you have registered one of these instances, it becomes an AWS OpsWorks Stacks instance and you can manage it in much the same way as instances that you create with AWS OpsWorks Stacks\.

## Apps<a name="welcome-classic-apps"></a>

You store applications and related files in a repository, such as an Amazon S3 bucket\. Each application is represented by an *app*, which specifies the application type and contains the information that is needed to deploy the application from the repository to your instances, such as the repository URL and password\. When you deploy an app, AWS OpsWorks Stacks triggers a Deploy event, which runs the Deploy recipes on the stack's instances\. 

You can deploy apps in the following ways:
+ Automatically—When you start instances, AWS OpsWorks Stacks automatically runs the instance's Deploy recipes\.
+ Manually—If you have a new app or want to update an existing one, you can manually run the online instances' Deploy recipes\.

You typically have AWS OpsWorks Stacks run the Deploy recipes on the entire stack, which allows the other layers' instances to modify their configuration appropriately\. However, you can limit deployment to a subset of instances if, for example, you want to test a new app before deploying it to every app server instance\.

## Customizing your Stack<a name="welcome-classic-customizing"></a>

AWS OpsWorks Stacks provides a variety of ways to customize layers to meet your specific requirements:
+ You can modify how AWS OpsWorks Stacks configures packages by overriding attributes that represent the various configuration settings, or by even overriding the templates used to create configuration files\.
+ You can extend an existing layer by providing your own recipes to perform tasks such as running scripts or installing and configuring nonstandard packages\.

All stacks can include one or more layers, which start with only a minimal set of recipes\. You add functionality to the layer by implementing recipes to handle tasks such as installing packages, deploying apps, and so on\. You package your custom recipes and related files in one or more *cookbooks* and store the cookbooks in a repository such Amazon S3 or Git\.

You can run recipes manually, but AWS OpsWorks Stacks also lets you automate the process by supporting a set of five *lifecycle events*:
+ **Setup** occurs on a new instance after it successfully boots\.
+ **Configure** occurs on all of the stack's instances when an instance enters or leaves the online state\.
+ **Deploy** occurs when you deploy an app\.
+ **Undeploy** occurs when you delete an app\.
+ **Shutdown** occurs when you stop an instance\. 

Each layer can have any number of recipes assigned to each event\. When a lifecycle event occurs on a layer's instance, AWS OpsWorks Stacks runs the associated recipes\. For example, when a Deploy event occurs on an app server instance, AWS OpsWorks Stacks runs the layer's Deploy recipes to download the app or perform related tasks\.

## Resource Management<a name="welcome-classic-resources"></a>

You can incorporate other AWS resources, such as [Elastic IP addresses](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html), into your stack\. You can use the AWS OpsWorks Stacks console or API to register resources with a stack, attach registered resources to or detach them from instances, and move resources from one instance to another\.

## Security and Permissions<a name="welcome-classic-security"></a>

AWS OpsWorks Stacks integrates with AWS Identity and Access Management \(IAM\) to provide robust ways of controlling how users access AWS OpsWorks Stacks, including the following:
+ How individual users can interact with each stack, such as whether they can create stack resources such as layers and instances, or whether they can use SSH or RDP to connect to a stack's Amazon EC2 instances\.
+ How AWS OpsWorks Stacks can act on your behalf to interact with AWS resources such as Amazon EC2 instances\.
+ How apps that run on AWS OpsWorks Stacks instances can access AWS resources such as Amazon S3 buckets\.
+ How to manage users' public SSH keys and RDP passwords and connect to an instance\.

## Monitoring and Logging<a name="welcome-classic-monitoring"></a>

AWS OpsWorks Stacks provides several features to help you monitor your stack and troubleshoot issues with your stack and any recipes\. For all stacks:
+ AWS OpsWorks Stacks provides a set of custom CloudWatch metrics for Linux stacks, which are summarized for your convenience on the **Monitoring** page\.

  AWS OpsWorks Stacks supports the standard CloudWatch metrics for Windows stacks\. You can monitor them with the CloudWatch console\. 
+ CloudTrail logs, which record API calls made by or on behalf of AWS OpsWorks Stacks in your AWS account\.
+ An event log, which lists all events in your stack\.
+ Chef logs that detail what transpired for each lifecycle event on each instance, such as which recipes were run and which errors occurred\.

Linux\-based stacks can also include a Ganglia master layer, which you can use to collect and display detailed monitoring data for the instances in your stack\.

## CLI, SDK, and AWS CloudFormation Templates<a name="welcome-classic-sdk"></a>

In addition to the console, AWS OpsWorks Stacks also supports a command\-line interface \(CLI\) and SDKs for multiple languages that can be used to perform any operation\. Consider these features:
+ The AWS OpsWorks Stacks CLI is part of the [AWS CLI](http://aws.amazon.com/documentation/cli/), and can be used to perform any operation from the command\-line\.

  The AWS CLI supports multiple AWS services and can be installed on Windows, Linux, or OS X systems\. 
+ AWS OpsWorks Stacks is included in [AWS Tools for Windows PowerShell](http://aws.amazon.com/documentation/powershell/) and can be used to perform any operation from a Windows PowerShell command line\.
+ The AWS OpsWorks Stacks SDK is included in the AWS SDKs, which can be used by applications implemented in: [Java](http://aws.amazon.com/documentation/sdkforjava/), [JavaScript](http://aws.amazon.com/documentation/sdkforjavascript/) \(browser\-based and Node\.js\), [\.NET](http://aws.amazon.com/documentation/sdkfornet/), [PHP](http://aws.amazon.com/documentation/sdkforphp/), [Python \(boto\)](http://boto.readthedocs.org/en/latest/), or [Ruby](http://aws.amazon.com/documentation/sdkforruby/)\.

You can also use AWS CloudFormation templates to provision stacks\. For some examples, see [AWS OpsWorks Snippets](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-opsworks.html)\.