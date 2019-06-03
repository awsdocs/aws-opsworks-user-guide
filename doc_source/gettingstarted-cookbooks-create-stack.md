# Step 2: Create the Stack and its Components<a name="gettingstarted-cookbooks-create-stack"></a>

Create an AWS OpsWorks Stacks stack and its components, which include a layer and an instance\. In later steps, you upload your cookbook to the instance and then run the cookbook's recipes on that instance\.

**To create the stack**

1. Using your AWS Identity and Access Management \(IAM\) user, sign in to the AWS OpsWorks Stacks console at [https://console\.aws\.amazon\.com/opsworks](https://console.aws.amazon.com/opsworks)\.

1. Do one of the following, if they apply:
   + If the **Welcome to AWS OpsWorks Stacks** page is displayed, choose **Add your first stack** or **Add your first AWS OpsWorks Stacks stack** \(both choices do the same thing\)\. The **Add stack** page is displayed\.
   + If the **OpsWorks Dashboard** page is displayed, choose **Add stack**\. The **Add Stack** page is displayed\.

1. Choose **Chef 12 stack**\.

1. In the **Stack name** box, type the stack's name, for example **MyCookbooksDemoStack**\. You can type a different name, but be sure to substitute it for `MyCookbooksDemoStack` throughout this walkthrough\.

1. For **Region**, choose **US West \(Oregon\)**\.

1. For **VPC**, do one of the following:
   + If a VPC is available, choose it\. For more information, see [Running a Stack in a VPC](workingstacks-vpc.md)\.
   + Otherwise, choose **No VPC**\.

1. For **Use custom Chef cookbooks**, choose **Yes**\.

1. For **Repository type**, choose **S3 Archive**\.
**Note**  
In the [Getting Started: Linux](gettingstarted-linux.md) walkthrough, you chose **Http Archive**\. Be sure to choose **S3 Archive** here instead\.

1. For **Repository URL**, type the path to your `opsworks_cookbook_demo.tar.gz` file in S3\. To get the path, in the S3 console, select the `opsworks_cookbook_demo.tar.gz` file\. On the **Properties** pane, copy the value of the **Link** field\. \(It should be similar to this: `https://s3.amazonaws.com/opsworks-demo-bucket/opsworks_cookbook_demo.tar.gz`\.\)

1. If your S3 bucket is private, which is the default, then for **Access key ID** and **Secret access key**, type the access key ID and secret access key of the IAM user that you are using for this walkthrough\. For more information, see [Editing Object Permissions](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/EditingPermissionsonanObject.html) and [Share an Object with Others](https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html)\.

1. Leave the defaults for the following:
   + **Default Availability Zone** \(**us\-west\-2a**\)
   + **Default operating system** \(**Linux** and **Amazon Linux 2016\.09**\)
   + **Default SSH key** \(**Do not use a default SSH key**\)
   + **Stack color** \(dark blue\)

1. Choose **Advanced**\.

1. For **IAM role**, do one of the following:
   + If **aws\-opsworks\-service\-role** is available, choose it\.
   + If **aws\-opsworks\-service\-role** is not available, choose **New IAM role**\.

1. For **Default IAM instance profile**, do one of the following:
   + If **aws\-opsworks\-ec2\-role** is available, choose it\.
   + If **aws\-opsworks\-ec2\-role** is not available, choose **New IAM instance profile**\.

1. Leave the defaults for the following:
   + **Default root device type** \(**EBS backed**\)
   + **Hostname theme** \(**Layer Dependent**\)
   + **OpsWorks Agent version** \(most recent version\)
   + **Custom Chef JSON** \(blank\)
   + **Security**, **Use OpsWorks security groups** \(**Yes**\)

1. Choose **Add stack**\. AWS OpsWorks Stacks creates the stack and displays the **MyCookbooksDemoStack** page\.

**To create the layer**

1. In the service navigation pane, choose **Layers**\. The **Layers** page is displayed\. 

1. Choose **Add a layer**\.

1. On the **OpsWorks** tab, for **Name**, type **MyCookbooksDemoLayer**\. You can type a different name, but be sure to substitute it for `MyCookbooksDemoLayer` throughout this walkthrough\.

1. For **Short name**, type **cookbooks\-demo**\. You can type a different name, but be sure to substitute it for `cookbooks-demo` throughout this walkthrough\.

1. Choose **Add layer**\. AWS OpsWorks Stacks adds the layer and displays the **Layers** page\.

**To create and start the instance**

1. In the service navigation pane, choose **Instances**\. The **Instances** page is displayed\.

1. Choose **Add an instance**\.

1. On the **New** tab, choose **Advanced**\. 

1. Leave the defaults for the following:
   + **Hostname** \(**cookbooks\-demo1**\)
   + **Size** \(**c3\.large**\)
   + **Subnet** \(*IP address* **us\-west\-2a**\)
   + **Scaling type** \(**24/7**\)
   + **SSH key** \(**Do not use a default SSH key**\)
   + **Operating system** \(**Amazon Linux 2016\.09**\)
   + **OpsWorks Agent version** \(**Inherit from stack**\)
   + **Tenancy** \(**Default \- Rely on VPC settings**\)
   + **Root device type** \(**EBS backed**\)
   + **Volume type** \(**General Purpose \(SSD\)**\)
   + **Volume size** \(**8**\)

1. Choose **Add instance**\.

1. For **MyCookbooksDemoLayer**, for **cookbooks\-demo1**, for **Actions**, choose **start**\. Do not proceed until **Status** changes to **online**\. This process might take several minutes, so be patient\.

You now have a stack, a layer, and an instance to which the cookbook was automatically copied from your S3 bucket\. In the [next step](gettingstarted-cookbooks-test-recipe.md), you will run and test the default recipe from within the cookbook on the instance\.