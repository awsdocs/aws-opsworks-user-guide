# Using the SDK for Ruby: Downloading Files from Amazon S3<a name="cookbooks-101-opsworks-s3"></a>

There are some tasks, such as interacting with AWS services, that cannot be handled with Chef resources\. For example, it is sometimes preferable to store files remotely and have a recipe download them to the instance\. You can use the [remote\_file](https://docs.chef.io/chef/resources.html#remote-file) resource to download files from remote servers\. However, if you want to store your files in an [Amazon S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html), `remote_file` can download those files only if the [ACL](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html) allows the operation\.

Recipes can use the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/) to access most AWS services\. This topic shows how to use the SDK for Ruby to download a file from an S3 bucket\.

**Note**  
For more information about how to use the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/) to handle encryption and decryption, see [AWS::S3::S3Object](http://docs.aws.amazon.com/AWSRubySDK/latest/AWS/S3/S3Object.html)\. Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**Topics**
+ [Using the SDK for Ruby on a Vagrant Instance](cookbooks-101-opsworks-s3-vagrant.md)
+ [Using the SDK for Ruby on an AWS OpsWorks Stacks Linux Instance](cookbooks-101-opsworks-s3-opsworks.md)
+ [Using the SDK for Ruby on an AWS OpsWorks Stacks Windows Instance](cookbooks-101-opsworks-s3-windows.md)