# Running a Stack in a VPC<a name="workingstacks-vpc"></a>

You can control user access to a stack's instances by creating it in a virtual private cloud \(VPC\)\. For example, you might not want users to have direct access to your stack's app servers or databases and instead require that all public traffic be channeled through an elastic load balancer\. 

The basic procedure for running a stack in a VPC is:

1. Create an appropriately configured VPC, by using the Amazon VPC console or API, or an AWS CloudFormation template\.

1. Specify the VPC ID when you create the stack\.

1. Launch the stack's instances in the appropriate subnet\.

The following briefly describes how VPCs work in AWS OpsWorks Stacks\.

**Important**  
If you use the VPC Endpoint feature, be aware that that each instance in the stack must be able to complete the following actions from Amazon Simple Storage Service \(Amazon S3\):  
Install the instance agent\.
Install assets, such as Ruby\. 
Upload Chef run logs\.
Retrieve stack commands\.
To enable these actions, you must ensure that the stack's instances have access to the following buckets that match the stack's region\. Otherwise, the preceding actions will fail\.  
For Chef 12 Linux and Chef 12\.2 Windows, the buckets are as follows\.  


| Agent Buckets | Asset Buckets | Log Buckets | DNA Buckets | 
| --- | --- | --- | --- | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  | 
For Chef 11\.10 and earlier versions for Linux, the buckets are as follows\. Chef 11\.4 stacks are not supported in regional endpoints outside the US East \(N\. Virginia\) Region\.  


| Agent Buckets | Asset Buckets | Log Buckets | DNA Buckets | 
| --- | --- | --- | --- | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html)  | 
For more information, see [VPC Endpoints](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html)\.

**Note**  
For AWS OpsWorks Stacks to connect to the VPC endpoints that you enable, you must also configure routing for your NAT or public IP, as the AWS OpsWorks Stacks agent still requires access to the public endpoint\.

**Topics**
+ [VPC Basics](#workingstacks-vpc-basics)
+ [Create a VPC for an AWS OpsWorks Stacks Stack](#workingstacks-vpc-create-vps)

## VPC Basics<a name="workingstacks-vpc-basics"></a>

For a detailed discussion of VPCs, see [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html)\. Briefly, a VPC consists of one or more *subnets*, each of which contains one or more instances\. Each subnet has an associated routing table that directs outbound traffic based on its destination IP address\.
+ Instances within a VPC can communicate with each other by default, regardless of their subnet\. However, changes to network access control lists \(ACLs\), security group policies, or using static IP addresses can break this communication\.
+ Subnets whose instances can communicate with the Internet are referred to as *public subnets*\.
+ Subnets whose instances can communicate only with other instances in the VPC and cannot communicate directly with the Internet are referred to as *private subnets*\.

AWS OpsWorks Stacks requires the VPC to be configured so that every instance in the stack, including instances in private subnets, has access to the following endpoints:
+ One of the AWS OpsWorks Stacks service endpoints listed in the "Region Support" section of [Getting Started with AWS OpsWorks Stacks](gettingstarted_intro.md)\.
+ One of the following instance service endpoints, used by the AWS OpsWorks Stacks agent\. The agent runs on managed customer instances to exchange data with the service\.
  + opsworks\-instance\-service\.us\-east\-2\.amazonaws\.com
  + opsworks\-instance\-service\.us\-east\-1\.amazonaws\.com
  + opsworks\-instance\-service\.us\-west\-1\.amazonaws\.com
  + opsworks\-instance\-service\.us\-west\-2\.amazonaws\.com
  + opsworks\-instance\-service\.ap\-south\-1\.amazonaws\.com
  + opsworks\-instance\-service\.ap\-northeast\-1\.amazonaws\.com
  + opsworks\-instance\-service\.ap\-northeast\-2\.amazonaws\.com
  + opsworks\-instance\-service\.ap\-southeast\-1\.amazonaws\.com
  + opsworks\-instance\-service\.ap\-southeast\-2\.amazonaws\.com
  + opsworks\-instance\-service\.ca\-central\-1\.amazonaws\.com
  + opsworks\-instance\-service\.eu\-central\-1\.amazonaws\.com
  + opsworks\-instance\-service\.eu\-west\-1\.amazonaws\.com
  + opsworks\-instance\-service\.eu\-west\-2\.amazonaws\.com
  + opsworks\-instance\-service\.eu\-west\-3\.amazonaws\.com
+ Amazon S3
+ Any package repositories that your operating system depends on, such as the Amazon Linux or Ubuntu Linux repositories\.
+ Your app and custom cookbook repositories\.

There are a variety of ways to configure a VPC to provide this connectivity\. The following is a simple example of how you could configure a VPC for an AWS OpsWorks Stacks app server stack\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/vpc.png)

This VPC has several components:

**Subnets**  
The VPC has two subnets, one public and one private\.  
+ The public subnet contains a load balancer and a network address translation \(NAT\) device, which can communicate with external addresses and with the instances in the private subnet\.
+ The private subnet contains the application servers, which can communicate with the NAT and load balancer in the public subnet but cannot communicate directly with external addresses\.

**Internet gateway**  
The Internet gateway allows instances with public IP addresses, such as the load balancer, to communicate with addresses outside the VPC\.

**Load balancer**  
The Elastic Load Balancing load balancer takes incoming traffic from users, distributes it to the app servers in the private subnet, and returns the responses to users\.

**NAT**  
The \(NAT\) device provides the app servers with limited Internet access, which is typically used for purposes such as downloading software updates from an external repository\. All AWS OpsWorks Stacks instances must be able to communicate with AWS OpsWorks Stacks and with the appropriate Linux repositories\. One way to handle this issue is to put a NAT device with an associated Elastic IP address in a public subnet\. You can then route outbound traffic from instances in the private subnet through the NAT\.  
A single NAT instance creates a single point of failure in your private subnet's outbound traffic\. You can improve reliability by configuring the VPC with a pair of NAT instances that take over for each other if one fails\. For more information, see [High Availability for Amazon VPC NAT Instances](http://aws.amazon.com/articles/6079781443936876)\. You can also use a NAT gateway\. For more information, see [NAT](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat.html) in the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

The optimal VPC configuration depends on your AWS OpsWorks Stacks stack\. The following are a few examples of when you might use certain VPC configurations\. For examples of other VPC scenarios, see [Scenarios for Using Amazon VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenarios.html)\.

**Working with one instance in a public subnet**  
If you have a single\-instance stack with no associated private resources—such as an Amazon RDS instance that should not be publicly accessible—you can create a VPC with one public subnet and put the instance in that subnet\. If you are not using a default VPC, you must have the instance's layer assign an Elastic IP address to the instance\. For more information, see [OpsWorks Layer Basics](workinglayers-basics.md)\. 

**Working with private resources**  
If you have resources that should not be publicly accessible, you can create a VPC with one public subnet and one private subnet\. For example, in a load\-balanced automatic scaling environment, you can put all the Amazon EC2 instances in the private subnet and the load balancer in a public subnet\. That way the Amazon EC2 instances cannot be directly accessed from the Internet; all incoming traffic must be routed through the load balancer\.  
The private subnet isolates the instances from Amazon EC2 direct user access, but they must still send outbound requests to AWS and the appropriate Linux package repositories\. To allow such requests you can, for example, use a network address translation \(NAT\) device with its own Elastic IP address and then route the instances' outbound traffic through the NAT\. You can put the NAT in the same public subnet as the load balancer, as shown in the preceding example\.   
+ If you are using a back\-end database such as an Amazon RDS instance, you can put those instances in the private subnet\. For Amazon RDS instances, you must specify at least two different subnets in different Availability Zones\. 
+ If you require direct access to instances in a private subnet—for example, you want to use SSH to log in to an instance—you can put a bastion host in the public subnet that proxies requests from the Internet\.

**Extending your own network into AWS**  
If you want to extend your own network into the cloud and also directly access the Internet from your VPC, you can create a VPN gateway\. For more information, see [Scenario 3: VPC with Public and Private Subnets and Hardware VPN Access](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario3.html)\. 

## Create a VPC for an AWS OpsWorks Stacks Stack<a name="workingstacks-vpc-create-vps"></a>

This section shows how to create a VPC for an AWS OpsWorks Stacks stack by using an example [AWS CloudFormation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) template\. You can download the template from [http://cloudformation\-templates\-us\-east\-1\.s3\.amazonaws\.com/OpsWorksinVPC\.template](http://cloudformation-templates-us-east-1.s3.amazonaws.com/OpsWorksinVPC.template)\. For more information on how to manually create a VPC like the one discussed in this topic, see [Scenario 2: VPC with Public and Private Subnets](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html)\. For details on how to configure routing tables, security groups, and so on, see the example template\.

**Note**  
By default, AWS OpsWorks Stacks displays subnet names by concatenating their CIDR range and Availability Zone, such as `10.0.0.1/24 - us-east-1b`\. To make the names more readable, create a tag for each subnet with **Key** set to **Name** and **Value** set to the subnet name\. AWS OpsWorks Stacks then appends the subnet name to the default name\. For example, the private subnet in the following example has a tag with **Name** set to **Private**, which OpsWorks displays as `10.0.0.1/24 us-east - 1b - Private`\. 

You can launch a VPC template using the AWS CloudFormation console with just a few steps\. The following procedure uses the example template to create a VPC in US East \(N\. Virginia\) Region\. For directions on how to use the template to create a VPC in other regions, see the [note](#vpc-note) that follows the procedure\.

**To create the VPC**

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/), select the **US East \(N\. Virginia\)** region, and choose **Create Stack**\.

1. On the **Select Template** page, select **Specify an Amazon S3 template URL** and paste the template URL: **http://cloudformation\-templates\-us\-east\-1\.s3\.amazonaws\.com/OpsWorksinVPC\.template**\. Choose **Continue**\.  
![\[CloudFormation Select Template page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/vpc_create_vpc.png)

   You can also launch this stack by opening [AWS CloudFormation Sample Templates](http://aws.amazon.com/cloudformation/aws-cloudformation-templates/), locating the AWS OpsWorks Stacks VPC template, and choosing **Launch Stack**\.

1. On the **Specify Parameters** page, accept the default values and choose **Continue**\.

1. On the **Add Tags** page, create a tag with **Key** set to **Name** and **Value** set to the VPC name\. This tag will make it easier to identify your VPC when you create an AWS OpsWorks Stacks stack\.

1. Choose **Continue** and then **Close** to launch the stack\.

<a name="vpc-note"></a>**Note: **You can create the VPC in other regions by using either of the following approaches\.
+ Go to [Using Templates in Different Regions](http://aws.amazon.com/cloudformation/aws-cloudformation-templates/#regions), choose the appropriate region, locate the AWS OpsWorks Stacks VPC template, and then choose **Launch Stack**\.
+ Copy the template file to your system, select the appropriate region in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/), and use the **Create Stack** wizard's **Upload a template to Amazon S3** option to upload the template from your system\.

The example template includes outputs that provide the VPC, subnet, and load balancer IDs that you will need to create the AWS OpsWorks Stacks stack\. You can see them by choosing the **Outputs** tab at the bottom of the AWS CloudFormation console window\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/vpc_cfn_outputs.png)