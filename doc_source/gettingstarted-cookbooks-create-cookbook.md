# Step 1: Create the Cookbook<a name="gettingstarted-cookbooks-create-cookbook"></a>

Start by creating a cookbook\. This cookbook won't do much to start\. However, it serves as a foundation for the rest of this walkthrough\.

**Note**  
This step demonstrates how to create a cookbook manually\. You can create a cookbook in less time with the Chef development kit \([Chef DK](https://docs.chef.io/#chef-dk-title)\) by running the command [https://docs.chef.io/ctl_chef.html#chef-generate-cookbook](https://docs.chef.io/ctl_chef.html#chef-generate-cookbook) on your local workstation\. However, this command creates several folders and files that you won't need for this walkthrough\.

**To create the cookbook**

1. On your local workstation, create a directory named `opsworks_cookbook_demo`\. You can use a different name, but be sure to substitute it for `opsworks_cookbook_demo` throughout this walkthrough\.

1. In the `opsworks_cookbook_demo` directory, use a text editor to create a file named `metadata.rb` with the following code to specify the cookbook's name \(for more information, go to [metadata\.rb](https://docs.chef.io/config_rb_metadata.html)\): 

   ```
   name "opsworks_cookbook_demo"
   ```

1. In the `opsworks_cookbook_demo` directory, create a subdirectory named `recipes`\. This subdirectory will contain all of the recipes that you will create for this walkthrough's cookbook\.

1. In the `recipes` directory, create a file named `default.rb`\. This file contains a recipe with the same name as the file, but without the file extension: `default`\. Add the following single line of code to the `default.rb` file\. This code is a one\-line recipe that displays a simple message in the log when the recipe runs:

   ```
   Chef::Log.info("********** Hello, World! **********")
   ```

1. At the terminal or command prompt, use the tar command to create a file named `opsworks_cookbook_demo.tar.gz`, which contains the `opsworks_cookbook_demo` directory and its contents\. For example:

   ```
   tar -czvf opsworks_cookbook_demo.tar.gz opsworks_cookbook_demo/
   ```

   You can use a different file name, but be sure to substitute it for `opsworks_cookbook_demo.tar.gz` throughout this walkthrough\.
**Note**  
When you create the `tar` file on Windows, the top directory must be the parent directory of the cookbook\. This walkthrough has been tested on Linux with the tar command provided by the `tar` package and on Windows with the tar command provided by [Git Bash](https://git-for-windows.github.io/)\. Using other commands or programs to create a compressed TAR \(\.tar\.gz\) file may not work as expected\.

1. Create an S3 bucket, or use an existing bucket\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

1. Upload the `opsworks_cookbook_demo.tar.gz` file to the S3 bucket\. For more information, see [Add an Object to a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html)\.

You now have a cookbook that you will use throughout this walkthrough\.

In the next step, you will create an AWS OpsWorks Stacks stack and its components—including an instance—that you will use later to upload your cookbook and to run the cookbook's recipes\.