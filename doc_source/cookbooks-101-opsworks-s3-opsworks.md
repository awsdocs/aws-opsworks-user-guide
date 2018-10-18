# Using the SDK for Ruby on an AWS OpsWorks Stacks Linux Instance<a name="cookbooks-101-opsworks-s3-opsworks"></a>

This topic describes how to use the SDK for Ruby on an AWS OpsWorks Stacks Linux instance to download a file from an Amazon S3 bucket\. AWS OpsWorks Stacks automatically installs the SDK for Ruby on every Linux instance\. However, when you create a service's client object, you must provide a suitable set of AWS credentials `AWS::S3.new` or the equivalent for other services\.

Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

 [Using the SDK for Ruby on a Vagrant Instance](cookbooks-101-opsworks-s3-vagrant.md) shows how to mitigate the risk of exposing your credentials by storing the credentials in the node object and referencing the attributes in your recipe code\. When you run recipes on an Amazon EC2 instance, you have an even better option, an [IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/WorkingWithRoles.html)\.

An IAM role works much like an IAM user\. It has an attached policy that grants permissions to use the various AWS services\. However, you assign a role to an Amazon EC2 instance rather than to an individual\. Applications running on that instance can then acquire the permissions granted by the attached policy\. With a role, credentials never appear in your code, even indirectly\. This topic describes how you can use an IAM role to run the recipe from [Using the SDK for Ruby on a Vagrant Instance](cookbooks-101-opsworks-s3-vagrant.md) on an Amazon EC2 instance\.

You could run this recipe with Test Kitchen using the kitchen\-ec2 driver, as described in [Example 9: Using Amazon EC2 Instances](cookbooks-101-basics-ec2.md)\. However, installing the SDK for Ruby on Amazon EC2 instances is somewhat complicated and not something you need to be concerned with for AWS OpsWorks Stacks\. All AWS OpsWorks Stacks Linux instances have the SDK for Ruby installed by default\. For simplicity, the example therefore uses an AWS OpsWorks Stacks instance\. 

The first step is to set up the IAM role\. This example takes the simplest approach, which is to use the Amazon EC2 role that AWS OpsWorks Stacks creates when you create your first stack\. It is named `aws-opsworks-ec2-role`\. However, AWS OpsWorks Stacks does not attach a policy to that role, so by default it grants no permissions\. You must attach a policy to the role that grants appropriate permissions, in this case, read\-only permissions for Amazon S3\. The procedure is much like the one you used to attach a policy to an IAM user in the preceding section\.

**To attach a policy to a role**

1. Open the [IAM console](https://console.aws.amazon.com/iam/) and click **Roles** in the navigation pane\.

1. Click `aws-opsworks-ec2-role` and under **Permissions**, select **Attach Policy**\.

1. Type **S3** in the **Policy Type** search box to display the Amazon S3 policies\. Select AmazonS3ReadOnlyAccess and click **Attach Policy**\.

You specify the role when you create or update a stack\. Set up a stack with a custom layer, as described in [Running a Recipe on a Linux Instance](cookbooks-101-opsworks-opsworks-instance.md), with one addition\. On the **Add Stack** page, confirm that **Default IAM instance profile** is set to **aws\-opsworks\-ec2\-role**\. AWS OpsWorks Stacks will then assign that role to all of the stack's instances\.

The procedure for setting up the cookbook is similar to the one used by [Running a Recipe on a Linux Instance](cookbooks-101-opsworks-opsworks-instance.md)\. The following is a brief summary; you should refer to that example for details\.

**To set up the cookbook**

1. Create a directory named `s3bucket_ops` and navigate to it\.

1. Create a `metadata.rb` file with the following content and save it to `s3bucket_ops`\.

   ```
   name "s3bucket_ops"
   version "0.1.0"
   ```

1. Create a `recipes` directory within `s3bucket_ops`\.

1. Create a `default.rb` file with the following recipe and save it to the `recipes` directory\.

   ```
   Chef::Log.info("******Downloading a file from Amazon S3.******")
   
   ruby_block "download-object" do
     block do
       require 'aws-sdk'
   
       s3 = AWS::S3.new
   
       myfile = s3.buckets['cookbook_bucket'].objects['myfile.txt']
       Dir.chdir("/tmp")
       File.open("myfile.txt", "w") do |f|
         f.syswrite(myfile.read)
         f.close
       end
     end
     action :run
   end
   ```

1. Create a `.zip` archive of `s3bucket_ops` and upload the archive to an Amazon S3 bucket\. For simplicity, [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html), then record the archive's URL for later use\. You can also store your cookbooks in a private Amazon S3 archive, or several other repository types\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

This recipe is similar the one used by the previous example, with the following exceptions\.
+ Because AWS OpsWorks Stacks has already installed the SDK for Ruby, the `chef_gem` resource has been deleted\.
+ The recipe does not pass any credentials to `AWS::S3.new`\.

  Credentials are automatically assigned to the application based on the instance's role\. For more information, see [Using IAM Roles for Amazon EC2 Instances with the AWS SDK for Ruby](http://docs.aws.amazon.com/AWSSdkDocsRuby/latest/DeveloperGuide/ruby-dg-roles.html)\.
+ The recipe uses `Chef::Log.info` to add a message to the Chef log\.

Create a stack for this example as follows\. You can also use an existing Windows stack\. Just update the cookbooks, as described later\.

**To create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and click **Add Stack**\.

1. Specify the following settings, accept the defaults for the other settings, and click **Add Stack**\.
   + **Name** – RubySDK
   + **Default SSH key** – An Amazon EC2 key pair

   If you need to create an Amazon EC2 key pair, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. Note that the key pair must belong to the same AWS region as the instance\. The example uses the default US West \(Oregon\) region\.

1. Click **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – S3Download
   + **Short name** – s3download

   Any layer type will actually work for Linux stacks, but the example doesn't require any of the packages that are installed by the other layer types, so a custom layer is the simplest approach\.

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the layer and [start it](workinginstances-starting.md)\.

You can now install and run the recipe

**To run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md), and specify the following settings\.
   + **Repository type** – **Http Archive**
   + **Repository URL** – The cookbook's archive URL that you recorded earlier\.

   Use the default values for the other settings and click **Save** to update the stack configuration\.

1. [Run the Update Custom Cookbooks stack command](workingstacks-commands.md), which installs the current version of your custom cookbooks on the stack's instances\. If an earlier version of your cookbooks is present, this command overwrites it\.

1. Execute the recipe by running the **Execute Recipes** stack command with **Recipes to execute** set to **s3bucket\_ops::default**\. This command initiates a Chef run, with a run list that consists of `s3bucket_ops::default`\.
**Note**  
You typically have AWS OpsWorks Stacks [run your recipes automatically ](workingcookbook-assigningcustom.md) by assigning them to the appropriate lifecycle event\. You can run such recipes by manually triggering the event\. You can use a stack command to trigger Setup and Configure events, and a [deploy command](workingapps-deploying.md) to trigger Deploy and Undeploy events\.

After the recipe runs successfully, you can verify it\.

**To verify s3bucket\_ops**

1. The first step is to examine the Chef log\. Your stack should have one instance named opstest1\. On the **Instances** page, click **show** in the instance's **Log** column to display the Chef log\. Scroll down and to find your log message near the bottom\.

   ```
   ...
   [2014-07-31T17:01:45+00:00] INFO: Storing updated cookbooks/opsworks_cleanup/attributes/customize.rb in the cache.
   [2014-07-31T17:01:45+00:00] INFO: Storing updated cookbooks/opsworks_cleanup/metadata.rb in the cache.
   [2014-07-31T17:01:46+00:00] INFO: ******Downloading a file from Amazon S3.******
   [2014-07-31T17:01:46+00:00] INFO: Processing template[/etc/hosts] action create (opsworks_stack_state_sync::hosts line 3)
   ...
   ```

1. [Use SSH to log in to the instance](workinginstances-ssh.md) and list the contents of `/tmp`\.