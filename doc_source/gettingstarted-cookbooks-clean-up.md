# Step 17: \(Optional\) Clean Up<a name="gettingstarted-cookbooks-clean-up"></a>

To prevent incurring additional charges to your AWS account, you can delete the AWS resources that were used for this walkthrough\. These AWS resources include the S3 bucket, the AWS OpsWorks Stacks stack, and the stack's components\. \(For more information, see [AWS OpsWorks Pricing](http://aws.amazon.com/opsworks/pricing/)\.\) However, you might want to keep using these AWS resources as you continue to learn more about AWS OpsWorks Stacks\. If you want to keep these AWS resources available, you have now completed this walkthrough, and you can skip to [Next Steps](gettingstarted-cookbooks-next-steps.md)\.

Content stored in the resources that you created for this walkthrough can contain personally\-identifying information\. If you no longer want this information to be stored by AWS, follow steps in this topic\.

**To delete the S3 bucket**
+ See [Delete the Amazon S3 Bucket](https://docs.aws.amazon.com/gettingstarted/latest/swh/getting-started-cleanup-s3.html)\.

**To delete the instance for the stack**

1. In the AWS OpsWorks Stacks console, in the service navigation pane, choose **Instances**\. The **Instances** page is displayed\.

1. For **MyCookbooksDemoLayer**, for **cookbooks\-demo1**, for **Actions**, choose **stop**\. When you see the confirmation message, choose **Stop**\.

1. The following changes occur over several minutes\. Do not proceed until all of the following have finished\.
   + **Status** changes from **online** to **stopping** and eventually to **stopped**\.
   + **online** changes from **1** to **0**\.
   + **shutting down** changes from **0** to **1** and eventually back to **0**\.
   + **stopped** eventually changes from **0** to **1**\.

1. For **Actions**, choose **delete**\. When you see the confirmation message, choose **Delete**\. AWS OpsWorks Stacks deletes the instance and displays **No instances**\.

**To delete the stack**

1. In the service navigation pane, choose **Stack**\. The **MyCookbooksDemoStack** page is displayed\.

1. Choose **Delete Stack**\. When you see the confirmation message, choose **Delete**\. AWS OpsWorks Stacks deletes the stack and displays the **Dashboard** page\.

Optionally, you can delete the IAM user and Amazon EC2 key pair that you used for this walkthrough, if you don't want to reuse them for access to other AWS services and EC2 instances\. For instructions, see [Deleting an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_manage.html#id_users_deleting) and [Deleting Your Key Pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#delete-key-pair)\.

You have now completed this walkthrough\. For more information, see see [Next Steps](gettingstarted-cookbooks-next-steps.md)\.