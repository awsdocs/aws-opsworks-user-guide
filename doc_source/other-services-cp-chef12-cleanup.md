# Step 8 \(Optional\): Clean up resources<a name="other-services-cp-chef12-cleanup"></a>

To help prevent unwanted charges to your AWS account, you can delete the AWS resources that you used for this walkthrough\. These AWS resources include the AWS OpsWorks Stacks stack, the IAM role and instance profile, and the pipeline that you created in CodePipeline\. However, you might want to keep using these AWS resources as you continue to learn more about AWS OpsWorks Stacks and CodePipeline\. If you want to keep these resources, you have finished this walkthrough\.

**To delete the app from the stack**

Because you did not create or apply the app as part of your AWS CloudFormation template, delete the Node\.js test app before you delete the stack in AWS CloudFormation\.

1. In the AWS OpsWorks Stacks console, in the service navigation pane, choose **Apps**\.

1. On the **Apps** page, select **Node\.js Demo App**, and then in **Actions**, choose **delete**\. When you are prompted to confirm, choose **Delete**\. AWS OpsWorks Stacks will delete the app\.

**To delete the stack**

Because you created the stack by running an AWS CloudFormation template, you can delete the stack, including the layer, instance, instance profile, and security group that the template created, in the AWS CloudFormation console\.

1. Open the AWS CloudFormation console\.

1. In the AWS CloudFormation console dashboard, select the stack you created\. On the **Actions** menu, choose **Delete Stack**\. When you are prompted to confirm, choose **Yes, Delete**\.

1. Wait for **DELETE\_COMPLETE** to appear in the **Status** column for the stack\.

**To delete the pipeline**

1. Open the CodePipeline console\.

1. In the CodePipeline dashboard, choose the pipeline you created for this walkthrough\.

1. On the pipeline page, choose **Edit**\.

1. On the **Edit** page, choose **Delete**\. When you are prompted to confirm, choose **Delete**\.