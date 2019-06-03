# Step 3: Add your app to AWS OpsWorks Stacks<a name="other-services-cp-chef11-addapp"></a>

Before you create a pipeline in CodePipeline, add the PHP test app to AWS OpsWorks Stacks\. When you create the pipeline, you will need to select the app that you've added to AWS OpsWorks Stacks\.

Have the Amazon S3 bucket link from step 10 of the preceding procedure ready\. You will need the link to the bucket in which you stored your test app to complete this procedure\.

**To add an app to AWS OpsWorks Stacks**

1. In the AWS OpsWorks Stacks console, open **MyStack**, and in the navigation pane, choose **Apps**\.

1. Choose **Add app**\.

1. On the **Add App** page, provide the following information: 

   1. Specify a name for your app\. This walkthrough uses the name `PHPTestApp`\.

   1. In the **Type** drop\-down list, choose **PHP**\.

   1. For **Data source type**, choose **None**\. This app does not require an external database or data source\.

   1. In the **Repository type** drop\-down list, choose **S3 Archive**\.

   1. In the **Repository URL** string box, paste the URL that you copied in step 10 of [Step 2: Upload app code to an Amazon S3 bucket](other-services-cp-chef11-s3.md)\. Your form should be similar to the following:  
![\[Add App page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_addappops.png)

1. You do not need to change any other settings in this form\. Choose **Add App**\.

1. When the **PHPTestApp** app appears in the list on the **Apps** page, continue to the next procedure, [Step 4: Create a pipeline in CodePipeline](other-services-cp-chef11-pipeline.md)\.