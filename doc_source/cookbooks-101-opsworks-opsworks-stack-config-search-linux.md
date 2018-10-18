# Using Search on a Linux Stack<a name="cookbooks-101-opsworks-opsworks-stack-config-search-linux"></a>

This example is based on a Linux stack with a single PHP application server\. It uses Chef search to obtain the server's public IP address and puts the address in a file in the `/tmp` directory\. It retrieves essentially the same information from the node object as [Obtaining Attribute Values Directly ](cookbooks-101-opsworks-opsworks-stack-config-node.md), but the code is much simpler and does not depend on the details of the stack configuration and deployment attribute structure\.

The following briefly summarizes how to create the stack for this example\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Note**  
If you have not run a custom recipe on an AWS OpsWorks Stacks instance before, you should first go through the [Running a Recipe on a Linux Instance](cookbooks-101-opsworks-opsworks-instance.md) example\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and click **Add Stack**\.

1. Specify the following settings, accept the defaults for the other settings, and click **Add Stack**\.
   + **Name** – SearchJSON
   + **Default SSH key** – An Amazon EC2 key pair

   If you need to create an Amazon EC2 key pair, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. Note that the key pair must belong to the same AWS region as the instance\. The example uses the US West \(Oregon\) region\.

1. Click **Add a layer** and [add a PHP App Server layer](workinglayers-custom.md) to the stack with default settings\.

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the layer and [start it](workinginstances-starting.md)\.

**To set up the cookbook**

1. Create a directory within `opsworks_cookbooks` named `searchjson` and navigate to it\.

1. Create a `metadata.rb` file with the following content and save it to `opstest`\.

   ```
   name "searchjson"
   version "0.1.0"
   ```

1. Create a `recipes` directory within `searchjson`\.

1. Create a `default.rb` file with the following recipe and save it to the `recipes` directory\.

   ```
   phpserver = search(:node, "layers:php-app").first
   Chef::Log.info("**********The public IP address is: '#{phpserver[:ip]}'**********")
   
   file "/tmp/ip_addresses" do
     content "#{phpserver[:ip]}"
     mode 0644
     action :create
   end
   ```

   Linux stacks support only the `node` search index\. The recipe uses this index to obtain a list of instances in the `php-app` layer\. Because the layer is known to have only one instance, the recipe simply assigns the first one to `phpserver`\. If the layer has multiple instances, you can enumerate them to retrieve the required information\. Each list item is a hash table containing a set of instance attributes\. The `ip` attribute is set to the instance's public IP address, so you can represent that address in the subsequent recipe code as `phpserver[:ip]`\.

   After adding a message to the Chef log, the recipe then uses a [https://docs.chef.io/chef/resources.html#file](https://docs.chef.io/chef/resources.html#file) resource to create a file named `ip_addresses`\. The `content` attribute is set to a string representation of `phpserver[:ip]`\. When Chef creates `ip_addresses`, it adds that string to the file\.

1. Create a `.zip` archive of `opsworks_cookbooks`, [Upload the archive to an Amazon S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html), [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html), and record the archive's URL\. It should look something like `https://s3.amazonaws.com/cookbook_bucket/opsworks_cookbooks.zip`\. For more information on cookbook repositories, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

You can now install the cookbook and run the recipe\.

**To run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md), and specify the following settings\.
   + **Repository type** – **Http Archive**
   + **Repository URL** – The cookbook archive URL that you recorded earlier

   Use the default values for the other settings and click **Save** to update the stack configuration\.

1. Edit the custom layer configuration and [assign `searchjson::default`](workingcookbook-assigningcustom.md) to the layer's Setup event\. AWS OpsWorks Stacks will run the recipe after the instance boots or if you explicitly trigger the Setup event\.

1. [Run the Update Custom Cookbooks stack command](workingstacks-commands.md), which installs the current version of your custom cookbook repository on the stack's instances\. If an earlier version of the repository is present, this command overwrites it\.

1. Execute the recipe by running the **Setup** stack command, which triggers a Setup event on the instance and runs `searchjson::default`\. Leave the **Running command setup page** open\.

After the recipe has run successfully, you can verify it\.

**To verify searchjson**

1. The first step is to examine the [Chef log](troubleshoot-debug-log.md) for the most recent Setup event\. On the **Running command setup page**, click **show** in the php\-app1 instance's **Log** column to display the log\. Scroll down to find your log message near the middle, which will look something like the following\.

   ```
   ...
   [2014-09-05T17:08:41+00:00] WARN: Previous bash[logdir_existence_and_restart_apache2]: ...
   [2014-09-05T17:08:41+00:00] WARN: Current  bash[logdir_existence_and_restart_apache2]: ...
   [2014-09-05T17:08:41+00:00] INFO: **********The public IP address is: '192.0.2.0'**********
   [2014-09-05T17:08:41+00:00] INFO: Processing directory[/etc/sysctl.d] action create (opsworks_initial_setup::sysctl line 1)
   ...
   ```

1. [Use SSH to log in to the instance](workinginstances-ssh.md) and list the contents of `/tmp`, which should include a file named `ip_addresses` that contains the IP address\.