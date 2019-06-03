# Step 6 \(Optional\): Update the app code to see CodePipeline redeploy your app automatically<a name="other-services-cp-chef11-update"></a>

When you make changes to code in apps or cookbooks that you have deployed by using CodePipeline, the updated artifacts will be deployed automatically by CodePipeline to your target instances \(in this case, to a target AWS OpsWorks Stacks stack\)\. This section shows you the automatic redeployment when you update the code in your sample PHP app\.

**To edit the code in the sample app**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Open the bucket in which you are storing your sample PHP app\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_editcodeS3.png)

1. Select the ZIP file that contains the app\. On the **Actions** menu, choose **Download**\.

1. In the dialog box, open the context \(right\-click\) menu, choose **Download**, and then save the ZIP file to a convenient location\. Choose **OK**\.

1. Extract the contents of the ZIP file to a convenient location\. You might need to change permissions on the extracted folder and its subfolders and contents to allow editing\. In the `opsworks-demo-php-simple-app-version1` folder, open the `index.php` file for editing\.

1. Search for the phrase, `Your PHP application is now running`\. Replace the text, `Your PHP application is now running` with `You've just deployed your first app to AWS OpsWorks with AWS CodePipeline,`\. Do not edit the variables\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_editheader.png)

1. Save and close the `index.php` file\.

1. Zip the `opsworks-demo-php-simple-app-version1` folder, and save the ZIP file to a convenient location\. Do not change the name of the ZIP file\.

1. Upload the new ZIP file to your Amazon S3 bucket\. In this walkthrough, the name of the bucket is `my-appbucket`\.

1. Open the CodePipeline console, and open your AWS OpsWorks Stacks pipeline \(**MyOpsWorksPipeline**\)\. Choose **Release Change**\.

   \(You can wait for CodePipeline to detect the code change from the updated version of the app in your Amazon S3 bucket\. To save you time, this walkthrough instructs you to simply choose **Release Change**\.\)

1. Observe as CodePipeline runs through the stages of the pipeline\. First, CodePipeline detects changes to the source artifact\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_cpupdatesource.png)

   CodePipeline pushes the updated code to your stack in AWS OpsWorks Stacks\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_updatestack.png)

1. When both stages of the pipeline have been successfully completed, open your stack in AWS OpsWorks Stacks \(**MyStack**\)\.

1. On the **MyStack** properties page, choose **Instances**\.

1. In the **Public IP** column, choose the public IP address of your instance to view the updated app's text\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_successedit.png)