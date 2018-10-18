# AWS Flow \(Ruby\) Layer<a name="workinglayers-awsflow"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

An AWS Flow \(Ruby\) layer is an AWS OpsWorks Stacks layer that provides a blueprint for instances that host [Amazon SWF](http://docs.aws.amazon.com/amazonswf/latest/developerguide/swf-welcome.html) activity and workflow workers\. The workers are implemented by using the [AWS Flow Framework for Ruby](http://docs.aws.amazon.com/amazonswf/latest/awsrbflowguide/welcome.html), which is a programming framework that simplifies the process of implementing a distributed asynchronous application while providing all the benefits of Amazon SWF\. It is ideal for implementing applications to address a broad range of scenarios, including business processes, media encoding, long\-running tasks, and background processing\.

The AWS Flow \(Ruby\) layer includes the following configuration settings\.

**RubyGems version**  
The framework's Gem version\. 

**Bundler version**  
The [Bundler](http://bundler.io/) version\.

**EC2 Instance profile**  
A user\-defined Amazon EC2 instance profile to be used by the layer's instances\. This profile must grant permissions for applications running on the layer's instances to access Amazon SWF\.

If your account does not have an appropriate profile, you can select **New profile with SWF access** to have AWS OpsWorks Stacks update the profile for or you can update it yourself by using the [IAM console](https://console.aws.amazon.com/iam/)\. You can then use the updated profile for all subsequent AWS Flow layers\. The following is a brief description of how to create the profile by using the IAM console\. For more information, see [Using IAM to Manage Access to Amazon SWF Resources](http://docs.aws.amazon.com/amazonswf/latest/developerguide/swf-dev-iam.html)\.

**Creating a profile for AWS Flow \(Ruby\) instances**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Click **Policies** in the navigation pane and click **Create Policy** to create a new customer\-managed policy\.

1. Click **Select** next to **Policy Generator** and then specify the following policy generator settings:
   + **Effect** – **Allow**\.
   + **AWS Service** – **Amazon Simple Workflow Service**\.
   + **Actions** – **All Actions \(\*\)**\. 
   + **Amazon Resource Name \(ARN\)** – An ARN that specifies which Amazon SWF domains the workers can access\. Type **\*** to provide access to all domains\.

   When you are finished, click **Add Statement**, **Next Step**, and then **Create Policy**\.

1. Click **Roles** in the navigation pane and click **Create New Role**\.

1. Specify the role name and click **Next Step**\. You cannot change the name after the role has been created\.

1. Select **AWS Service Roles** and then select **Amazon EC2**\.

1. Click **Customer Managed Policies** from the **Policy Type** list and select the policy that you created earlier\.

1. Specify this profile when you create an AWS Flow \(Ruby\) layer in AWS OpsWorks Stacks\.

**Note**  
For more information on how to use the AWS Flow \(Ruby\) layer, including a detailed walkthrough that describes how to deploy a sample application, see [Tutorial: Hello AWS OpsWorks\!](http://docs.aws.amazon.com/amazonswf/latest/awsrbflowguide/hello-opsworks.html)\.