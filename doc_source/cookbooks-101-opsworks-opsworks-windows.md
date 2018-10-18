# Running a Recipe on a Windows Instance<a name="cookbooks-101-opsworks-opsworks-windows"></a>

This topic is basically an abbreviated version of [Running a Recipe on a Linux Instance](cookbooks-101-opsworks-opsworks-instance.md), which shows you how to run a recipe on a Windows stack\. We recommend that you go through [Running a Recipe on a Linux Instance](cookbooks-101-opsworks-opsworks-instance.md) first, because it provides a more detailed discussion, most of which is relevant to either type of operating system\.

For a description of how to run recipes on AWS OpsWorks Stacks Linux instances, see [Running a Recipe on a Linux Instance](cookbooks-101-opsworks-opsworks-instance.md)\.

**Topics**
+ [Enabling RDP Access](#cookbooks-101-opsworks-opsworks-windows-rdp)
+ [Creating and Running the Recipe](#cookbooks-101-opsworks-opsworks-windows-run-recipe)
+ [Executing the Recipe Automatically](#cookbooks-101-opsworks-opsworks-windows-event)

## Enabling RDP Access<a name="cookbooks-101-opsworks-opsworks-windows-rdp"></a>

Before you start, if you have not done so already, you must set up a security group with an inbound rule that allows RDP access for your instances \. You will need that group when you create the stack\.

When you create the first stack in a region, AWS OpsWorks Stacks creates a set of security groups\. They include one named something like `AWS-OpsWorks-RDP-Server`, which AWS OpsWorks Stacks attaches to all Windows instances to allow RDP access\. However, by default, this security group does not have any rules, so you must add an inbound rule to allow RDP access to your instances\.

**To allow RDP access**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/v2/), set it to the stack's region, and choose **Security Groups** from the navigation pane\.

1. Choose **AWS\-OpsWorks\-RDP\-Server**, choose the **Inbound** tab, and choose **Edit**\.

1. Add a rule with the following settings:
   + **Type** – **RDP**
   + **Source** – The permissible source IP addresses\.

     You typically allow inbound RDP requests from your IP address or a specified IP address range \(typically your corporate IP address range\)\.

**Note**  
As described later, you also must edit user permissions to authorize RDP access for regular users\.

For more information, see [Logging In with RDP](workinginstances-rdp.md)\. 

## Creating and Running the Recipe<a name="cookbooks-101-opsworks-opsworks-windows-run-recipe"></a>

The following briefly summarizes how to create a stack for this example\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and choose **Add Stack**\. Specify the following settings, accept the defaults for the other settings, and choose **Add Stack**\.
   + **Name** – WindowsRecipeTest
   + **Region** – US West \(Oregon\)

     This example will work in any region, but we recommend using US West \(Oregon\) for tutorials\.
   + **Default operating system** – Microsoft Windows Server 2012 R2

1. Choose **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – RecipeTest
   + **Short name** – recipetest

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the RecipeTest layer and [start it](workinginstances-starting.md)\.

   AWS OpsWorks Stacks automatically assigns `AWS-OpsWorks-RDP-Server` to this instance, which allows authorized users to log in to the instance\.

1. Choose **Permissions** and then **Edit**, and choose **SSH/RDP** and **sudo/admin**\. Regular users need this authorization in addition to the `AWS-OpsWorks-RDP-Server` security group to log in to the instance\. 
**Note**  
You can also log in as Administrator, but it requires a different procedure\. For more information, see [Logging In with RDP](workinginstances-rdp.md)\.

While the instance is starting up—it usually takes several minutes—you can create the cookbook\. The recipe for this example creates a data directory, and is basically the recipe from [Example 3: Creating Directories](cookbooks-101-basics-directories.md), modified for Windows\.

**Note**  
When implementing cookbooks for AWS OpsWorks Stacks Windows instances, you use a somewhat different directory structure than you do when implementing cookbooks for AWS OpsWorks Stacks Linux instances\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\. 

**To set up the cookbook**

1. Create a directory named `windowstest` and navigate to it\.

1. Create a `metadata.rb` file with the following content and save it to `windowstest`\.

   ```
   name "windowstest"
   version "0.1.0"
   ```

1. Create a `recipes` directory within `windowstest`\.

1. Create a `default.rb` file with the following recipe and save it to the `recipes` directory\.

   ```
   Chef::Log.info("******Creating a data directory.******")
   
   directory 'C:\data' do
     rights :full_control, 'instance_name\username'
     inherits false
     action :create
   end
   ```

   Replace *username* with your user name\.

1. Put the cookbook in a repository\.

   To install your cookbook on an AWS OpsWorks Stacks instance, you must store it in a repository and provide AWS OpsWorks Stacks with the information required to download the cookbook to the instance\. You can store Windows cookbooks as an archive file in an S3 bucket or in a Git repository\. This example uses an S3 bucket, so you must create a \.zip archive of the `windowstest` directory\. For more information on cookbook repositories, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

1. [Upload the archive to an S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html), [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html), and record the archive's URL\. It should look something like `https://s3-us-west-2.amazonaws.com/opsworks-windows/opsworks_cookbooks.zip`\. You can also use a private archive, but a public archive is sufficient for this example and somewhat easier to work with\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

You can now install the cookbook and run the recipe\.

**To run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md) and specify the following settings\.
   + **Repository type** – **S3 Archive**
   + **Repository URL** – The cookbook archive URL that you recorded earlier

   Accept the default values for the other settings and choose **Save** to update the stack configuration\.

1. [Run the Update Custom Cookbooks stack command](workingstacks-commands.md), which installs the current version of your custom cookbooks on the stack's instances, including online instances\. If an earlier version of your cookbooks is present, this command overwrites it\.

1. After Update Custom Cookbooks is finished, execute the recipe by running the [**Execute Recipes** stack command](workingstacks-commands.md) with **Recipes to execute** set to **windowstest::default**\. This command initiates a Chef run, with a run list that consists of your recipe\.

After the recipe runs successfully, you can verify it\.

**To verify windowstest**

1. Examine the [Chef log](troubleshoot-debug-log.md)\. Choose **show** in the opstest1 instance's **Log** column to display the log\. Scroll down and you will see your log message near the bottom\.

   ```
   ...
   [2014-07-31T17:01:45+00:00] INFO: Storing updated cookbooks/opsworks_cleanup/attributes/customize.rb in the cache.
   [2014-07-31T17:01:45+00:00] INFO: Storing updated cookbooks/opsworks_cleanup/metadata.rb in the cache.
   [2014-07-31T17:01:46+00:00] INFO: ******Creating a data directory.******
   [2014-07-31T17:01:46+00:00] INFO: Processing template[/etc/hosts] action create (opsworks_stack_state_sync::hosts line 3)
   ...
   ```

1. Choose **Instances**, choose **rdp** in the instance's **Actions** column, and request an RDP password with a suitable expiration time\. Copy the DNS name, user name, and password\. You can then can use that information with an RDP client, such as the Windows Remote Desktop Connection client, to log in to the instance and verify that `c:\data` exists\. For more information, see [Logging In with RDP](workinginstances-rdp.md)\.

**Note**  
If your recipe isn't working properly, see [Troubleshooting and Fixing Recipes](cookbooks-101-opsworks-opsworks-instance.md#cookbooks-101-opsworks-opsworks-instance-bugs) for troubleshooting tips; most of them also apply to Windows instances\. If you want to test your fix by editing the recipe on the instance, look for your cookbook in the `C:\chef\cookbooks` directory, where AWS OpsWorks Stacks installs custom cookbooks\.

## Executing the Recipe Automatically<a name="cookbooks-101-opsworks-opsworks-windows-event"></a>

The **Execute Recipes** command is a convenient way to test custom recipes, which is why it is used in most of these examples\. However, in practice you typically run recipes at standard points in an instance's lifecycle, such as after the instance finishes booting or when you deploy an app\. AWS OpsWorks Stacks simplifies running recipes on your instance by supporting a set of [lifecycle events](workingcookbook-events.md) for each layer: Setup, Configure, Deploy, Undeploy, and Shutdown\. You can have AWS OpsWorks Stacks run a recipe automatically on a layer's instances by assigning the recipe to the appropriate lifecycle event\.

You would typically create directories as soon as an instance finishes booting, which corresponds to the Setup event\. The following shows how to run the example recipe at setup, using the same stack that you created earlier in the example\. You can use the same procedure for the other events\.

**To automatically run a recipe at setup**

1. Choose **Layers** in the navigation pane and then choose the pencil icon next to the RecipeTest layer's **Recipes** link\.

1. Add **windowstest::default** to the layer's **Setup** recipes, choose **\+** to add it to the layer, and choose **Save** to save the configuration\.

1. Choose **Instances**, add another instance to the layer, and start it\.

   The instance should be named `recipetest2`\. After it finishes booting, AWS OpsWorks Stacks will run `windowstest::default`\.

1. After the `recipetest2` instance is online, verify that `c:\data` is present\.

**Note**  
If you have assigned recipes to the Setup, Configure, or Deploy events, you can also run them manually by using a [stack command](workingstacks-commands.md) \(Setup and Configure\) or a [deploy command](workingapps-deploying.md) \(Deploy\) to trigger the event\. Note that if you have multiple recipes assigned to an event, these commands run all of them\.