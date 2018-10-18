# Step 6: Deploy and Run PhotoApp<a name="using-s3-run"></a>

For this example, the application has also been implemented for you and is stored in a [public GitHub repository](https://github.com/amazonwebservices/opsworks-demo-php-photo-share-app)\. You just need to add the app to the stack, deploy it to the application servers, and run it\. 

**To add the app to the stack and deploy it to the application servers**

1. Open the **Apps** page and choose **Add an app**\.

1. On the **Add App** page, do the following:
   + Set **Name** to **PhotoApp**\.
   + Set **App type** to **PHP**\.
   + Set **Document root** to **web**\.
   + Set **Repository type** to **Git**\.
   + Set **Repository URL** to **git://github\.com/awslabs/opsworks\-demo\-php\-photo\-share\-app\.git**\.
   + Choose **Add App** to accept the defaults for the other settings\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/photoapp_walkthrough_app.png)

1. On the **Apps** page, choose **deploy** in the PhotoApp app's **Actions** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/photoapp_walkthrough_deploy.png)

1. Accept the defaults and choose **Deploy** to deploy the app to the server\.

To run PhotoApp, go to the **Instances** page and choose the PHP App Server instance's public IP address\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/photoapp_walkthrough_run.png)

You should see the following user interface\. Choose **Add a Photo** to store a photo on the Amazon S3 bucket and the metadata in the back\-end data store\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/photoapp_walkthrough_ui.png)