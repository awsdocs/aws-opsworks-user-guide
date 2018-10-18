# Step 2: Create a PHP App Server Stack<a name="using-s3-stack"></a>

The stack consists of two layers, PHP App Server and MySQL, each with one instance\. The application stores photos on an Amazon S3 bucket, but uses the MySQL instance as a back\-end data store to hold metadata for each photo\.

Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**To create the stack**

1. Create a new stack—named **PhotoSite** for this example—and add a PHP App Server layer\. You can use the default settings for both\. For more information, see [Create a New Stack](workingstacks-creating.md) and [Creating an OpsWorks Layer ](workinglayers-basics-create.md)\.

1. On the **Layers** page, for PHP App Server, choose **Security** and then choose **Edit**\.

1. In the **Layer Profile** section, select the instance profile name that you recorded earlier, after launching the AppServer AWS CloudFormation stack\. It will be something like `AppServer-AppServerInstanceProfile-1Q3KD0DNMGB90`\. AWS OpsWorks Stacks assigns this profile to all of the layer's Amazon EC2 instances, which grants permission to access your Amazon S3 bucket to applications running on the layer's instances \.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/photoapp_profile.png)

1. Add an instance to the PHP App Server layer and start it\. For more information on how to add and start instances, see [Adding an Instance to a Layer](workinginstances-add.md)\.

1. Add a MySQL layer to the stack, add an instance, and start it\. You can use default settings for both the layer and instance\. In particular, the MySQL instance doesn't need to access the Amazon S3 bucket, so it can use the standard AWS OpsWorks Stacks instance profile, which is selected by default\.