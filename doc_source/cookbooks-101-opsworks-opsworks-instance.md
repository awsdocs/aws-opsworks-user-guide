# Running a Recipe on an AWS OpsWorks Stacks Linux Instance<a name="cookbooks-101-opsworks-opsworks-instance"></a>

Test Kitchen and Vagrant provide a simple and efficient way to implement cookbooks, but to verify that a cookbook's recipes will run correctly in production, you must run them on an AWS OpsWorks Stacks instance\. This topic describes how to install a custom cookbook on an AWS OpsWorks Stacks Linux instance and run a simple recipe\. The topic also provides some tips for efficiently fixing recipe bugs\.

For a description of how to run recipes on Windows instances, see [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md)\.

**Topics**
+ [Creating and Running the Recipe](#opsworks-opsworks-instance-create)
+ [Executing the Recipe Automatically](#cookbooks-101-opsworks-opsworks-instance-events)
+ [Troubleshooting and Fixing Recipes](#cookbooks-101-opsworks-opsworks-instance-bugs)

## Creating and Running the Recipe<a name="opsworks-opsworks-instance-create"></a>

First, you need to create a stack\. The following briefly summarizes how to create a stack for this example\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**To create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and click **Add Stack**\.

1. Specify the following settings, accept the defaults for the other settings, and click **Add Stack**\.
   + **Name** – OpsTest
   + **Default SSH key** – An Amazon EC2 key pair

   If you need to create an Amazon EC2 key pair, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. Note that the key pair must belong to the same AWS region as the instance\. The example uses the default US West \(Oregon\) region\.

1. Click **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – OpsTest
   + **Short name** – opstest

   Any layer type will actually work for Linux stacks, but the example doesn't require any of the packages that are installed by the other layer types, so a custom layer is the simplest approach\.

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the layer and [start it](workinginstances-starting.md)\.

While the instance is starting up—it usually takes several minutes—you can create the cookbook\. This example will use a slightly modified version of the recipe from [Conditional Logic](cookbooks-101-basics-ruby.md#cookbooks-101-basics-ruby-conditional), which creates a data directory whose name depends on the platform\.

**To set up the cookbook**

1. Create a directory within `opsworks_cookbooks` named `opstest` and navigate to it\.

1. Create a `metadata.rb` file with the following content and save it to `opstest`\.

   ```
   name "opstest"
   version "0.1.0"
   ```

1. Create a `recipes` directory within `opstest`\.

1. Create a `default.rb` file with the following recipe and save it to the `recipes` directory\.

   ```
   Chef::Log.info("******Creating a data directory.******")
   
   data_dir = value_for_platform(
     "centos" => { "default" => "/srv/www/shared" },
     "ubuntu" => { "default" => "/srv/www/data" },
     "default" => "/srv/www/config"
   )
   
   directory data_dir do
     mode 0755
     owner 'root'
     group 'root'
     recursive true
     action :create
   end
   ```

   Notice that the recipe logs a message, but it does so by calling `Chef::Log.info`\. You aren't using Test Kitchen for this example, so the `log` method isn't very useful\. `Chef::Log.info` puts the message into the Chef log, which you can read after the Chef run is finished\. AWS OpsWorks Stacks provides an easy way to view these logs, as described later\.
**Note**  
Chef logs usually contain a lot of routine and relatively uninteresting information\. The '\*' characters bracketing the message text make it easier to spot\.

1. Create a `.zip` archive of `opsworks_cookbooks`\. To install your cookbook on an AWS OpsWorks Stacks instance, you must store it in a repository and provide AWS OpsWorks Stacks with the information required to download the cookbook to the instance\. You can store your cookbooks in any of several supported repository types\. This example stores an archive file containing the cookbooks in an Amazon S3 bucket\. For more information on cookbook repositories, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.
**Note**  
For simplicity, this example just archives the entire `opsworks_cookbooks` directory\. However, it means that AWS OpsWorks Stacks will download all the cookbooks in `opsworks_cookbooks` to the instance, even though you will use only one of them\. To install only the example cookbook, create another parent directory and move `opstest` to that directory\. Then create a `.zip` archive of the parent directory and use it instead of `opsworks_cookbooks.zip`\.   
Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

1. [Upload the archive to an Amazon S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html), [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html), and record the archive's URL\. It should look something like `https://s3.amazonaws.com/cookbook_bucket/opsworks_cookbooks.zip`\.

You can now install the cookbook and run the recipe\.

**To run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md), and specify the following settings\.
   + **Repository type** – **S3 Archive**
   + **Repository URL** – The cookbook archive URL that you recorded earlier

   Use the default values for the other settings and click **Save** to update the stack configuration\.

1. [Run the Update Custom Cookbooks stack command](workingstacks-commands.md), which installs the current version of your custom cookbooks on the stack's instances\. If an earlier version of your cookbooks is present, this command overwrites it\.

1. Execute the recipe by running the **Execute Recipes** stack command with **Recipes to execute** set to **opstest::default**\. This command initiates a Chef run, with a run list that consists of `opstest::default`\.

After the recipe runs successfully, you can verify it\.

**To verify opstest**

1. The first step is to examine the [Chef log](troubleshoot-debug-log.md)\. Click **show** in the opstest1 instance's **Log** column to display the log\. Scroll down and you will see your log message near the bottom\.

   ```
   ...
   [2014-07-31T17:01:45+00:00] INFO: Storing updated cookbooks/opsworks_cleanup/attributes/customize.rb in the cache.
   [2014-07-31T17:01:45+00:00] INFO: Storing updated cookbooks/opsworks_cleanup/metadata.rb in the cache.
   [2014-07-31T17:01:46+00:00] INFO: ******Creating a data directory.******
   [2014-07-31T17:01:46+00:00] INFO: Processing template[/etc/hosts] action create (opsworks_stack_state_sync::hosts line 3)
   ...
   ```

1. [Use SSH to log in to the instance](workinginstances-ssh.md) and list the contents of `/srv/www/`\.

If you followed all the steps, you will see `/srv/www/config` rather than the `/srv/www/shared` directory you were expecting\. The following section provides some guidelines for quickly fixing such bugs\.

## Executing the Recipe Automatically<a name="cookbooks-101-opsworks-opsworks-instance-events"></a>

The **Execute Recipes** command is a convenient way to test custom recipes, which is why it is used in most of these examples\. However, in practice you typically run recipes at standard points in an instance's lifecycle, such as after the instance finishes booting or when you deploy an app\. AWS OpsWorks Stacks simplifies running recipes on your instance by supporting a set of [lifecycle events](workingcookbook-events.md) for each layer: Setup, Configure, Deploy, Undeploy, and Shutdown\. You can have AWS OpsWorks Stacks run a recipe automatically on a layer's instances by assigning the recipe to the appropriate lifecycle event\.

You would typically create directories as soon as an instance finishes booting, which corresponds to the Setup event\. The following shows how to run the example recipe at setup, using the same stack that you created earlier in the example\. You can use the same procedure for the other events\.

**To automatically run a recipe at setup**

1. Choose **Layers** in the navigation pane and then chose the pencil icon next to the OpsTest layer's **Recipes** link\.

1. Add **opstest::default** to the layer's **Setup** recipes, click **\+** to add it to the layer, and choose **Save** to save the configuration\.

1. Choose **Instances**, add another instance to the layer, and start it\.

   The instance should be named `opstest2`\. After it finishes booting, AWS OpsWorks Stacks will run `opstest::default`\.

1. After the `opstest2` instance is online, verify that `/srv/www/shared` is present\.

**Note**  
If you have assigned recipes to the Setup, Configure, or Deploy events, you also run them manually by using a [stack command](workingstacks-commands.md) \(Setup and Configure\) or a [deploy command](workingapps-deploying.md) \(Deploy\) to trigger the event\. Note that if you have multiple recipes assigned to an event, these commands run all of them\.

## Troubleshooting and Fixing Recipes<a name="cookbooks-101-opsworks-opsworks-instance-bugs"></a>

If you aren't getting the expected results, or your recipes don't even run successfully, troubleshooting typically starts by examining the Chef log\. It contains a detailed description of the run and includes any inline log messages from your recipes\. The logs are particularly useful if your recipe simply failed\. When that happens, Chef logs the error, including a stack trace\. 

If the recipe was successful, as it was for this example, the Chef log often isn't much help\. In this case, you can figure out the problem by just taking a closer look at the recipe, the first few lines in particular:

```
Chef::Log.info("******Creating a data directory.******")

data_dir = value_for_platform(
  "centos" => { "default" => "/srv/www/shared" },
  "ubuntu" => { "default" => "/srv/www/data" },
  "default" => "/srv/www/config"
)
...
```

CentOS is a reasonable stand\-in for Amazon Linux when you are testing recipes on Vagrant, but now you are running on an actual Amazon Linux instance\. The platform value for Amazon Linux is `amazon`, which isn't included in the `value_for_platform` call, so the recipe creates `/srv/www/config` by default\. For more information on troubleshooting, see [Debugging and Troubleshooting Guide](troubleshoot.md)\.

Now that you have identified the problem, you need to update the recipe and verify the fix\. You could go back to the original source files, update `default.rb`, upload a new archive to Amazon S3, and so on\. However, that process can be a bit tedious and time consuming\. The following shows a much quicker approach that is especially useful for simple recipe bugs like the one in the example: edit the recipe on the instance\. 

**To edit a recipe on an instance**

1. Use SSH to log in to the instance and then run `sudo su` to elevate your privileges\. You need root privileges to access the cookbook directories\.

1. AWS OpsWorks Stacks stores your cookbook in `/opt/aws/opsworks/current/site-cookbooks`, so navigate to `/opt/aws/opsworks/current/site-cookbooks/opstest/recipes`\.
**Note**  
AWS OpsWorks Stacks also stores a copy of your cookbooks in `/opt/aws/opsworks/current/merged-cookbooks`\. Don't edit that cookbook\. When you execute the recipe, AWS OpsWorks Stacks copies the cookbook from `.../site-cookbooks` to `.../merged-cookbooks`, so any changes you make in `.../merged-cookbooks` will be overwritten\.

1. Use a text editor on the instance to edit `default.rb`, and replace `centos` with `amazon`\. Your recipe should now look like the following\.

   ```
   Chef::Log.info("******Creating a data directory.******")
   
   data_dir = value_for_platform(
     "amazon" => { "default" => "/srv/www/shared" },
     "ubuntu" => { "default" => "/srv/www/data" },
     "default" => "/srv/www/config"
   )
   ...
   ```

To verify the fix, execute the recipe by running the **Execute Recipe** stack command again\. The instance should now have a `/srv/www/shared` directory\. If you need to make further changes to a recipe, you can run **Execute Recipe** as often as you like; you don't need to stop and restart the instance each time you run the command\. When you are satisfied that the recipe is working correctly, don't forget to update the code in your source cookbook\.

**Note**  
If you have assigned your recipe to a lifecycle event so AWS OpsWorks Stacks runs it automatically, you can always use **Execute Recipe** to rerun the recipe\. You can also rerun the recipe as many times as you want without restarting the instance by using the AWS OpsWorks Stacks console to manually trigger the appropriate event\. However, this approach runs all of the event's recipes\. Here's a reminder:  
Use a [stack command](workingstacks-commands.md) to trigger Setup or Configure events\.
Use a [deploy command](workingapps-deploying.md) to trigger Deploy or Undeploy events\.