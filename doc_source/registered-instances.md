# Using Computing Resources Created Outside of AWS OpsWorks Stacks<a name="registered-instances"></a>

**Note**  
This feature is supported only for Linux stacks\.

[Instances](workinginstances.md) describes how to use AWS OpsWorks Stacks to create and manage groups of Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. You can also incorporate Linux computing resources into a stack that was created outside of AWS OpsWorks Stacks:
+ Amazon EC2 instances that you created directly by using the Amazon EC2 console, CLI, or API\.
+ *On\-premises* instances running on your own hardware, including instances running in virtual machines\.

These computing resources become AWS OpsWorks Stacks\-managed instances, and you can manage them much like regular AWS OpsWorks Stacks instances:
+ **Manage user permissions** – You can use [AWS OpsWorks Stacks user management](opsworks-security-users.md) to specify which users are allowed to access your stacks, which actions they are allowed to perform on the stack's instances, and whether they have SSH access and sudo privileges\. 
+ **Automate tasks** – You can have AWS OpsWorks Stacks run custom Chef recipes to perform tasks such as executing scripts on any or all of a stack's instances with a single command\.

  If you assign the instance to a [layer](workinglayers.md), AWS OpsWorks Stacks automatically runs a specified set of Chef recipes on the instance at key points in its [lifecycle](workingcookbook-events.md), including your custom recipes\. Note that you can assign registered Amazon EC2 instances to [custom layers](workinglayers-custom.md) only\.
+ **Manage resources** – A stack lets you group and manage resources in an AWS region, and the OpsWorks dashboard shows the status of your stacks across all regions\.
+ **Install packages** – You can use Chef recipes to install packages on any instance in a stack\.
+ **Update the operating system** – AWS OpsWorks Stacks provides a simple way to install operating system security patches and updates on a stack's instances\.
+ **Deploy applications** – AWS OpsWorks Stacks deploys applications consistently to all of the stack's application server instances\.
+ **Monitoring** – AWS OpsWorks Stacks creates custom [CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatch.html) metrics to monitor all of your stack's instances\.

For pricing information, see [AWS OpsWorks Pricing](https://aws.amazon.com/opsworks/stacks/pricing/)\.

Following is the basic procedure for working with a registered instance\.

1. Register the instance with a stack\.

   The instance is now part of the stack and managed by AWS OpsWorks Stacks\.

1. Optionally, assign the instance to a layer\.

   This step lets you take full advantage of AWS OpsWorks Stacks management functionality\. You can assign registered on\-premises instances to any layer; registered Amazon EC2 instances can be assigned to custom layers only\.

1. Use AWS OpsWorks Stacks to manage the instance\.

1. When you no longer need the instance in the stack, deregister it, which removes the instance from AWS OpsWorks Stacks\.

The following sections describe this process in detail\.

**Topics**
+ [Registering an Instance with an AWS OpsWorks Stacks Stack](registered-instances-register.md)
+ [Managing Registered Instances](registered-instances-manage.md)
+ [Assigning a Registered Instance to a Layer](registered-instances-assign.md)
+ [Unassigning a Registered Instance](registered-instances-unassign.md)
+ [Deregistering a Registered Instance](registered-instances-deregister.md)
+ [Registered Instance Life Cycle](registered-instances-lifecycle.md)