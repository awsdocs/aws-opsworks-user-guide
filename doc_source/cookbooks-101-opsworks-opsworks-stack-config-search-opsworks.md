# Using AWS OpsWorks Stacks\-Specific Search Indexes on Windows Stacks<a name="cookbooks-101-opsworks-opsworks-stack-config-search-opsworks"></a>

**Note**  
This example assumes that you have already done the [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md) example\. If not, you should do that example first\. In particular, it describes how to enable RDP access to your instances\.

AWS OpsWorks Stacks provides the following search indexes in addition to `node`: 
+ `aws_opsworks_stack` – The stack configuration\.
+ `aws_opsworks_layer` – The stack's layer configurations\.
+ `aws_opsworks_instance` – The stack's instance configurations\.
+ `aws_opsworks_app` – The stack's app configurations\.
+ `aws_opsworks_user` – The stack's user configurations\.
+ `aws_opsworks_rds_db_instance` – Connection information for registered RDS instances\.

These indexes include some standard Chef attributes, but are primarily intended for retrieving AWS OpsWorks Stacks\-specific attributes\. For example `aws_opsworks_instance` includes a `status` attribute that provides the instance's status, such as `online`\. 

**Note**  
The recommended practice is to use `node` when possible to keep your recipes consistent with standard Chef usage\. For an example, see [Using the node Search Index on Windows Stacks](cookbooks-101-opsworks-opsworks-stack-config-search-node.md)\.

This example shows how to use the AWS OpsWorks Stacks indexes to retrieve the value of an AWS OpsWorks Stacks\-specific attribute\. It is based on a simple Windows stack with a custom layer that has one instance\. It uses Chef search to obtain the instance's AWS OpsWorks Stacks ID and puts the results in the Chef log\.

The following briefly summarizes how to create a stack for this example\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and choose **\+ Stack**\. Specify the following settings, accept the defaults for the other settings, and choose **Add Stack**\.
   + **Name** – IDSearch
   + **Region** – US West \(Oregon\)

     This example will work in any region, but we recommend using US West \(Oregon\) for tutorials\.
   + **Default operating system** – Microsoft Windows Server 2012 R2

1. Choose **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – IDCheck
   + **Short name** – idcheck

1. [Add a 24/7 t2\.micro instance](workinginstances-add.md) with default settings to the IDCheck layer and [start it](workinginstances-starting.md)\. It will be named iptest1\.

   AWS OpsWorks Stacks automatically assigns `AWS-OpsWorks-RDP-Server` to this instance\. [Enabling RDP Access](cookbooks-101-opsworks-opsworks-windows.md#cookbooks-101-opsworks-opsworks-windows-rdp) explains how to add an inbound rule to this security group that allows authorized users to log in to the instance\.

1. Choose **Permissions** and then **Edit**, and choose **SSH/RDP** and **sudo/admin**\. Regular users need this authorization in addition to the `AWS-OpsWorks-RDP-Server` security group to log in to the instance\. 
**Note**  
You can also log in as Administrator, but it requires a different procedure\. For more information, see [Logging In with RDP](workinginstances-rdp.md)\.

**To set up the cookbook**

1. Create a directory named `idcheck` and navigate to it\.

1. Create a `metadata.rb` file with the following content and save it to `opstest`\.

   ```
   name "idcheck"
   version "0.1.0"
   ```

1. Create a `recipes` directory within `idcheck` and add a `default.rb` file to the directory that contains the following recipe\.

   ```
   windowsserver = search(:aws_opsworks_instance, "hostname:idcheck*").first
   Chef::Log.info("**********The public IP address is: '#{windowsserver[:instance_id]}'**********")
   ```

   The recipe uses Chef search with an `aws_opsworks_instance` search index to obtain the [instance attributes](data-bag-json-instance.md) of each instance in the stack with a hostname that starts with `idcheck`\. If you use the default theme, which creates hostnames by appending integers to the layer's short name, this query will return every instance in the IDCheck layer\. For this example, the layer is known to have only one instance, so the recipe simply assigns the first one to `windowsserver`\. For multiple instances, you can get the complete list and then enumerate them\.

   The recipe takes advantage of the fact that there is only one instance in the stack with this hostname, so the first result is the correct one\. If your stack has multiple instances, searching on other attributes might return more than one result\. For a list of instance attributes, see [Instance Data Bag \(aws\_opsworks\_instance\)](data-bag-json-instance.md)\.

   The instance attributes are basically a hash table, and the instance's AWS OpsWorks Stacks ID is assigned to the `instance_id` attribute, so you can refer to the ID as `windowsserver[:instance_id]`\. The recipe inserts the corresponding string into the message and adds it to the Chef log\.

1. Create a `.zip` archive of the `ipaddress` cookbook, [Upload the archive to an Amazon S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html), and record the archive's URL\. It should look something like `https://s3-us-west-2.amazonaws.com/windows-cookbooks/ipaddress.zip`\. For more information on cookbook repositories, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

You can now install the cookbook and run the recipe\.

**To install the cookbook and run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md) and specify the following settings\.
   + **Repository type** – **S3 Archive**
   + **Repository URL** – The cookbook archive URL that you recorded earlier

   Accept the default values for the other settings, and choose **Save** to update the stack configuration\.

1. [Run the Update Custom Cookbooks stack command](workingstacks-commands.md), which installs the current version of your custom cookbooks on the stack's instances, including online instances\. If an earlier version of your cookbooks is present, this command overwrites it\.

1. After Update Custom Cookbooks is finished, execute the recipe by running the [**Execute Recipes** stack command](workingstacks-commands.md) with **Recipes to execute** set to **idcheck::default**\. This command initiates a Chef run, with a run list that consists of your recipe\. Leave the execute\_recipes page open\.

After the recipe has run successfully, you can verify it by examining the [Chef log](troubleshoot-debug-log.md) for the most recent execute\_recipes event\. On the **Running command execute\_recipes page**, choose **show** in the iptest1 instance's **Log** column to display the log\. Scroll down to find your log message near the bottom, which will look something like the following\.

```
...
[2015-05-13T20:03:47+00:00] INFO: Storing updated cookbooks/nodesearch/recipes/default.rb in the cache.
[2015-05-13T20:03:47+00:00] INFO: Storing updated cookbooks/nodesearch/metadata.rb in the cache.
[2015-05-13T20:03:47+00:00] INFO: **********The instance ID is: 'i-8703b570'**********
[2015-05-13T20:03:47+00:00] INFO: Chef Run complete in 0.312518 seconds 
...
```