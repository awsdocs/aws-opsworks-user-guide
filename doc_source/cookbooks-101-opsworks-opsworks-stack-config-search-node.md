# Using the node Search Index on Windows Stacks<a name="cookbooks-101-opsworks-opsworks-stack-config-search-node"></a>

**Note**  
This example assumes that you have already done the [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md) example\. If not, you should do that example first\. In particular, it describes how to enable RDP access to your instances\.

This example is based on a Windows stack with a single custom layer and one instance\. It uses Chef search with the `node` search index to obtain the server's public IP address and puts the address in a file in the `C:\tmp` directory\. The following briefly summarizes how to create the stack for this example\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and choose **Add Stack**\.

1. Specify the following settings, accept the defaults for the other settings, and choose **Add Stack**\.
   + **Name** – NodeSearch
   + **Region** – US West \(Oregon\)

     This example will work in any region, but we recommend using US West \(Oregon\) for tutorials\.
   + **Default operating system** – Microsoft Windows Server 2012 R2

1. Choose **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – IPTest
   + **Short name** – iptest

1. [Add a 24/7 t2\.micro instance](workinginstances-add.md) with default settings to the IPTest layer and [start it](workinginstances-starting.md)\. It will be named iptest1\.

   AWS OpsWorks Stacks automatically assigns `AWS-OpsWorks-RDP-Server` to this instance, which allows authorized users to log in to the instance\.

1. Choose **Permissions** and then **Edit**, and select **SSH/RDP** and **sudo/admin**\. Regular users need this authorization in addition to the `AWS-OpsWorks-RDP-Server` security group to log in to the instance\. 
**Note**  
You also can log in as Administrator, but it requires a different procedure\. For more information, see [Logging In with RDP](workinginstances-rdp.md)\.

**To set up the cookbook**

1. Create a directory named `nodesearch` and navigate to it\.

1. Create a `metadata.rb` file with the following content and save it to `opstest`\.

   ```
   name "nodesearch"
   version "0.1.0"
   ```

1. Create a `recipes` directory within `nodesearch`\.

1. Create a `default.rb` file with the following recipe and save it to the `recipes` directory\.

   ```
   directory 'C:\tmp' do
     rights :full_control, 'Everyone'
     recursive true
     action :create
   end
   
   windowsserver = search(:node, "hostname:iptest*").first
   Chef::Log.info("**********The public IP address is: '#{windowsserver[:ipaddress]}'**********")
   
   file 'C:\tmp\addresses.txt' do
     content "#{windowsserver[:ipaddress]}"
     rights :full_control, 'Everyone'
     action :create
   end
   ```

   The recipe does the following:

   1. Uses a directory resource to create a `C:\tmp` directory for the file\.

      For more information on this resource, see [Example 3: Creating Directories](cookbooks-101-basics-directories.md)\.

   1. Uses Chef search with the `node` search index to obtain a list of nodes \(instances\) with a hostname that starts with `iptest`\.

      If you use the default theme, which creates hostnames by appending integers to the layer's short name, this query will return every instance in the IPTest layer\. For this example, the layer is known to have only one instance, so the recipe simply assigns the first one to `windowsserver`\. For multiple instances, you can get the complete list and then enumerate them\.

   1. Adds a message with the IP address to the Chef log for this run\.

      The `windowsserver` object is a hash table whose `ipaddress` attribute is set to the instance's public IP address, so you can represent that address in the subsequent recipe code as `windowsserver[:ipaddress]`\. The recipe inserts the corresponding string into the message and adds it to the Chef log\.

   1. Uses the `file` resource to create a file with the IP address named `C:\tmp\addresses.txt`\.

      The resource's `content` attribute specifies content to be added to the file, which is the public IP address in this case\. 

1. Create a `.zip` archive of `nodesearch`, [Upload the archive to an S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html), [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html), and record the archive's URL\. It should look something like `https://s3.amazonaws.com/cookbook_bucket/nodesearch.zip`\. For more information on cookbook repositories, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

You can now install the cookbook and run the recipe\.

**To install the cookbook and run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md) and specify the following settings\.
   + **Repository type** – **S3 Archive**
   + **Repository URL** – The cookbook archive URL that you recorded earlier

   Accept the default values for the other settings, and choose **Save** to update the stack configuration\.

1. [Run the Update Custom Cookbooks stack command](workingstacks-commands.md), which installs the current version of your custom cookbooks on the stack's instances, including online instances\. If an earlier version of your cookbooks is present, this command overwrites it\.

1. After Update Custom Cookbooks has finished, execute the recipe by running the [**Execute Recipes** stack command](workingstacks-commands.md) with **Recipes to execute** set to **nodesearch::default**\. This command initiates a Chef run, with a run list that consists of your recipe\. Leave the execute\_recipes page open\.

After the recipe has run successfully, you can verify it\.

**To verify nodesearch**

1. Examine the [Chef log](troubleshoot-debug-log.md) for the most recent execute\_recipes event\. On the **Running command execute\_recipes page**, choose **show** in the iptest1 instance's **Log** column to display the log\. Scroll down to find your log message near the bottom, which will look something like the following\.

   ```
   ...
   [2015-05-13T18:55:47+00:00] INFO: Storing updated cookbooks/nodesearch/recipes/default.rb in the cache.
   [2015-05-13T18:55:47+00:00] INFO: Storing updated cookbooks/nodesearch/metadata.rb in the cache.
   [2015-05-13T18:55:47+00:00] INFO: **********The public IP address is: '192.0.0.1'**********
   [2015-05-13T18:55:47+00:00] INFO: Processing directory[C:\tmp] action create (nodesearch::default line 1)
   [2015-05-13T18:55:47+00:00] INFO: Processing file[C:\tmp\addresses.txt] action create (nodesearch::default line 10) 
   ...
   ```

1. [Use RDP to log in to the instance](workinginstances-rdp.md) and examine the contents of `C:\tmp\addresses.txt`\.