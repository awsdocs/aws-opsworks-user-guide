# Running a Windows PowerShell Script<a name="cookbooks-101-opsworks-opsworks-powershell"></a>

**Note**  
These examples assume that you have already done the [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md) example\. If not, you should do that example first\. In particular, it describes how to [enable RDP access](cookbooks-101-opsworks-opsworks-windows.md#cookbooks-101-opsworks-opsworks-windows-rdp) to your instances\.

One way to have a recipe perform tasks on a Windows instance—especially tasks that do not have a corresponding Chef resource—is to have the recipe run a Windows PowerShell script\. This section introduces you to the basics by describing how to use a Windows PowerShell script to install a Windows feature\.

The [https://docs.chef.io/chef/resources.html#powershell-script](https://docs.chef.io/chef/resources.html#powershell-script) resource runs Windows PowerShell cmdlets on an instance\. The following example uses a [Install\-WindowsFeature cmdlet](https://technet.microsoft.com/en-us/library/hh849795.aspx) to install an XPS viewer on the instance\. 

The following briefly summarizes how to create a stack for this example\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and choose **Add Stack**\. Specify the following settings, accept the defaults for the other settings, and click **Add Stack**\.
   + **Name** – PowerShellTest
   + **Region** – US West \(Oregon\)

     This example will work in any region, but we recommend using US West \(Oregon\) for tutorials\.
   + **Default operating system** – Microsoft Windows Server 2012 R2

1. Choose **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – PowerShell
   + **Short name** – powershell

1. [Add a 24/7 instance](workinginstances-add.md) to with default settings to the PowerShell layer and [start it](workinginstances-starting.md)\.

1. Choose **Permissions** and then **Edit**, and select **SSH/RDP** and **sudo/admin**\. You need this authorization in addition to the `AWS-OpsWorks-RDP-Server` security group to log in to the instance as a regular user\.

While the instance is starting up—it usually takes several minutes—you can create the cookbook\. The recipe for this example creates a data directory, and is basically the recipe from [Example 3: Creating Directories](cookbooks-101-basics-directories.md), modified for Windows\.

**To set up the cookbook**

1. Create a directory named `powershell` and navigate to it\.

1. Create a `metadata.rb` file with the following content and save it to `windowstest`\.

   ```
   name "powershell"
   version "0.1.0"
   ```

1. Create a `recipes` directory within the `powershell` directory\.

1. Create a `default.rb` file with the following recipe and save it to the `recipes` directory\.

   ```
   Chef::Log.info("******Installing XPS.******")
   
   powershell_script "Install XPS Viewer" do
     code <<-EOH
       Install-WindowsFeature XPS-Viewer
     EOH
     guard_interpreter :powershell_script
     not_if "(Get-WindowsFeature -Name XPS-Viewer).installed"
   end
   ```
   + The `powershell_script` resource runs a cmdlet to install the XPS viewer\.

     This example runs only one cmdlet, but the `code` block can contain any number of command lines\.
   + The `guard_interpreter` attribute directs Chef to use the 64\-bit version of Windows PowerShell\.
   + The `not_if` guard attribute ensures that Chef does not install the feature if it has already been installed\.

1. Create a `.zip` archive of the `powershell` directory\.

1. [Upload the archive to an Amazon S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html), [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html), and record the archive's URL\. It should look something like `https://s3-us-west-2.amazonaws.com/opsworks-windows/powershell.zip`\. You can also use a private archive, but a public archive is sufficient for this example, and somewhat easier to work with\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

You can now install the cookbook and run the recipe\.

**To run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md) and specify the following settings\.
   + **Repository type** – **S3 Archive**
   + **Repository URL** – The cookbook archive URL that you recorded earlier

   Accept the default values for the other settings and choose **Save** to update the stack configuration\.

1. [Run the **Update Custom Cookbooks **stack command](workingstacks-commands.md) to install the current version of your custom cookbooks on the instance\. 

1. After **Update Custom Cookbooks** has finished, execute the recipe by running the [**Execute Recipes** stack command](workingstacks-commands.md) with **Recipes to execute** set to **powershell::default**\. 

**Note**  
This example uses **Execute Recipes** for convenience, but you typically have AWS OpsWorks Stacks [run your recipes automatically ](workingcookbook-assigningcustom.md) by assigning them to the appropriate lifecycle event\. You can run such recipes by manually triggering the event\. You can use a stack command to trigger Setup and Configure events, and a [deploy command](workingapps-deploying.md) to trigger Deploy and Undeploy events\.

After the recipe runs successfully, you can verify it\.

**To verify the powershell recipe**

1. Examine the [Chef log](troubleshoot-debug-log.md)\. Click **show** in the powershell1 instance's **Log** column to display the log\. Scroll down and you will see your log message near the bottom\.

   ```
   ...
   [2015-04-27T18:12:09+00:00] INFO: Storing updated cookbooks/powershell/metadata.rb in the cache.
   [2015-04-27T18:12:09+00:00] INFO: ******Installing XPS.******
   [2015-04-27T18:12:09+00:00] INFO: Processing powershell_script[Install XPS Viewer] action run (powershell::default line 3)
   [2015-04-27T18:12:09+00:00] INFO: Processing powershell_script[Guard resource] action run (dynamically defined)
   [2015-04-27T18:12:42+00:00] INFO: powershell_script[Install XPS Viewer] ran successfully 
   ...
   ```

1. [Use RDP to log in to the instance](workinginstances-rdp.md) and open the **Start** menu\. XPS Viewer should be listed with **Windows Accessories**\.