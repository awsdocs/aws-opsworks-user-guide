# Step 2: Create a Simple Application Server Stack \- Chef 11<a name="gettingstarted-simple"></a>

A basic application server stack consists of a single application server instance with a public IP address to receive user requests\. Application code and any related files are stored in a separate repository and deployed from there to the server\. The following diagram illustrates such a stack\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch_2.png)

The stack has the following components:
+ A *layer*, which represents a group of instances and specifies how they are to be configured\.

  The layer in this example represents a group of PHP App Server instances\.
+ An *instance*, which represents an Amazon EC2 instance\.

  In this case, the instance is configured to run a PHP app server\. Layers can have any number of instances\. AWS OpsWorks Stacks also supports several other app servers\. For more information, see [Application Server Layers](workinglayers-servers.md)\.
+ An *app*, which contains the information required to install an application on the application server\.

  The code is stored in a remote repository, such as Git repository or an Amazon S3 bucket\.

The following sections describe how to use the AWS OpsWorks Stacks console to create the stack and deploy the application\. You can also use an AWS CloudFormation template to provision a stack\. For an example template that provisions the stack described in this topic, see [AWS OpsWorks Snippets](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-opsworks.html)\.

**Topics**
+ [Step 2\.1: Create a Stack \- Chef 11](gettingstarted-simple-stack.md)
+ [Step 2\.2: Add a PHP App Server Layer \- Chef 11](gettingstarted-simple-layer.md)
+ [Step 2\.3: Add an Instance to the PHP App Server Layer \- Chef 11](gettingstarted-simple-instance.md)
+ [Step 2\.4: Create and Deploy an App \- Chef 11](gettingstarted-simple-app.md)