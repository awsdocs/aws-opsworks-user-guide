# Using an Amazon S3 Bucket<a name="gettingstarted.walkthrough.photoapp"></a>

Applications often use an Amazon Simple Storage Service \(Amazon S3\) bucket to store large items such as images or other media files\. Although AWS OpsWorks Stacks does not provide integrated support for Amazon S3, you can easily customize a stack to allow your application to use Amazon S3 storage\. This topic walks you through the basic process of providing Amazon S3 access to applications, using a Linux stack with a PHP application server as an example\. The basic principles also apply to Windows stacks\.

Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**Topics**
+ [Step 1: Create an Amazon S3 Bucket](using-s3-bucket.md)
+ [Step 2: Create a PHP App Server Stack](using-s3-stack.md)
+ [Step 3: Create and Deploy a Custom Cookbook](using-s3-cookbook.md)
+ [Step 4: Assign the Recipes to LifeCycle Events](using-s3-events.md)
+ [Step 5: Add Access Information to the Stack Configuration and Deployment Attributes](using-s3-json.md)
+ [Step 6: Deploy and Run PhotoApp](using-s3-run.md)