# Step 2: Create a Stack<a name="gettingstarted-linux-create-stack"></a>

You will use the AWS OpsWorks Stacks console to create a stack\. A *stack* is a collection of instances and related AWS resources that have a common purpose and that you want to manage together\. \(For more information, see [Stacks](workingstacks.md)\.\) For this walkthrough, there is only one instance\.

Before you begin, complete the [prerequisites](gettingstarted-linux-prerequisites.md) if you haven't already\.

**To create the stack**

1. Using your IAM user, sign in to the AWS OpsWorks Stacks console at [https://console\.aws\.amazon\.com/opsworks](https://console.aws.amazon.com/opsworks)\.

1. Do any of the following, if they apply:
   + If the **Welcome to AWS OpsWorks Stacks** page is displayed, choose **Add your first stack** or **Add your first AWS OpsWorks Stacks stack** \(both choices do the same thing\)\. The **Add stack** page is displayed\.
   + If the **OpsWorks Dashboard** page is displayed, choose **Add stack**\. The **Add stack** page is displayed\.

1. With the **Add stack** page displayed, choose **Chef 12 stack** if it is not already chosen for you\.

1. In the **Stack name** box, type a name, for example **MyLinuxDemoStack**\. \(You can type a different name, but be sure to substitute it for `MyLinuxDemoStack` throughout this walkthrough\.\)

1. For **Region**, choose **US West \(Oregon\)**\.

1. For **VPC**, do one of the following:
   + If a VPC is available, choose it\. \(For more information, see [Running a Stack in a VPC](workingstacks-vpc.md)\.\)
   + Otherwise, choose **No VPC**\.

1. For **Default operating system**, choose **Linux** and **Ubuntu 18\.04 LTS**\.

1. For **Use custom Chef cookbooks**, choose **Yes**\.

1. For **Repository type**, choose **Http Archive**\.

1. For **Repository URL**, type **https://s3\.amazonaws\.com/opsworks\-demo\-assets/opsworks\-linux\-demo\-cookbooks\-nodejs\.tar\.gz**

1. Leave the defaults for the following:
   + **Default Availability Zone** \(**us\-west\-2a**\)
   + **Default SSH key** \(**Do not use a default SSH key**\)
   + **User name** \(blank\)
   + **Password** \(blank\)
   + **Stack color** \(dark blue\)

1. Choose **Advanced**\.

1. For **IAM role**, do one of the following \(for more information, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\):
   + If **aws\-opsworks\-service\-role** is available, choose it\.
   + If **aws\-opsworks\-service\-role** is not available, choose **New IAM role**\.

1. For **Default IAM instance profile**, do one of the following \(for more information, see [Specifying Permissions for Apps Running on EC2 instances](opsworks-security-appsrole.md)\):
   + If **aws\-opsworks\-ec2\-role** is available, choose it\.
   + If **aws\-opsworks\-ec2\-role** is not available, choose **New IAM instance profile**\.

1. For **API endpoint region**, choose the regional API endpoint with which you want the stack to be associated\. If you want the stack to be in the US West \(Oregon\) region within the US East \(N\. Virginia\) regional endpoint, choose **us\-east\-1**\. If you want the stack to be both in the US West \(Oregon\) region and associated with the US West \(Oregon\) regional endpoint, choose **us\-west\-2**\.
**Note**  
The US East \(N\. Virginia\) regional endpoint includes older AWS regions for backward compatibility, but it is a best practice to choose the regional endpoint that is closest to where you manage AWS\. For more information, see [Region Support](gettingstarted_intro.md#gettingstarted-intro-region)\.

1. Leave the defaults for the following:
   + **Default root device type** \(**EBS backed**\)
   + **Hostname theme** \(**Layer Dependent**\)
   + **OpsWorks Agent version** \(most recent version\)
   + **Custom JSON** \(blank\)
   + **Use OpsWorks security groups** \(**Yes**\)

1. Your results should match the following screenshots except for perhaps **VPC**, **IAM role**, and **Default IAM instance profile**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-add-stack-top-console.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-add-stack-bottom-console.png)

1. Choose **Add Stack**\. AWS OpsWorks Stacks creates the stack and displays the **MyLinuxDemoStack** page\.

You now have a stack with the correct settings for this walkthrough\.

In the [next step](gettingstarted-linux-add-layer.md), you will add a layer to the stack\.