# Installing a Package on a Windows Instance<a name="cookbooks-101-opsworks-install-software-package"></a>

**Note**  
This example assumes that you have already done the [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md) example\. If not, you should do that example first\. In particular, it describes how to enable RDP access to your instances\.

If your software comes in an installer package, such as an MSI, you must download the file to the instance and then run it\. This example shows how to implement a cookbook to install an MSI package, the Python runtime, including how to define associated environment variables\. For more information on how to install Windows features such as IIS, see [Installing a Windows Feature: IIS](cookbooks-101-opsworks-install-software-feature.md)\.

**To set up the cookbook**

1. Create a directory named `installpython` and navigate to it\.

1. Add a `metadata.rb` file to `installpython` with the following content\.

   ```
   name "installpython"
   version "0.1.0"
   ```

1. Add `recipes` and `files` directories to `installpython` and add a `default` directory to files\.

1. Download a Python package from [Python Releases for Windows](https://www.python.org/downloads/windows/) to the cookbook's `files\default` directory\. This example installs the Windows x86\-64 version of Python 3\.5\.0a3, which uses an MSI installer named `python-3.4.3.amd64.msi`\.

1. Add a file named `default.rb` to the `recipes` directory with the following recipe code\.

   ```
   directory 'C:\tmp' do
     rights :full_control, 'Everyone'
     recursive true
     action :create
   end
   
   cookbook_file 'C:\tmp\python-3.4.3.amd64.msi' do
     source "python-3.4.3.amd64.msi"
     rights :full_control, 'Everyone'
     action :create
   end
   
   windows_package 'python' do
     source 'C:\tmp\python-3.4.3.amd64.msi'
     action :install
   end
   
   env "PATH" do
     value 'c:\python34'
     delim ";"
     action :modify
   end
   ```

   The recipe does the following:

   1. Uses a [directory](https://docs.chef.io/chef/resources.html#directory) resource to create a `C:\tmp` directory\.

      For more information on this resource, see [Example 3: Creating Directories](cookbooks-101-basics-directories.md)\.

   1. Uses a [cookbook\_file](https://docs.chef.io/chef/resources.html#cookbook-file) resource to copy the installer from the cookbook's `files\default` directory to `C:\tmp`\.

      For more information on this resource, see [Installing a File from a Cookbook](cookbooks-101-basics-files.md#cookbooks-101-basics-files-cookbook_file)\.

   1. Uses a [windows\_package](https://docs.chef.io/chef/resources.html#windows-package) resource to run the MSI installer, which installs Python to `c:\python34`\.

      The installer creates the required directories and installs the files, but does not modify the system's `PATH` environment variable\.

   1. Uses an [env](https://docs.chef.io/chef/resources.html#env) resource to add `c:\python34` to the system path\.

      You use the env resource to define environment variables\. In this case, the recipe allows you to easily run Python scripts from the command line by adding `c:\python34` to the path\.
      + The resource name specifies the environment variable's name, `PATH` for this example\.
      + The `value` attribute specifies the variable's value, `c:\\python34` for this example \(you need to escape the `\` character\)\.
      + The `:modify` action prepends the specified value to the variable's current value\.
      + The `delim` attribute specifies a delimiter that separates the new value from the existing value, which is `;` for this example\.

1. Create a `.zip` archive of `installpython`, upload the archive to an S3 bucket, and make it public\. Record the archive's URL for later use\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

Create a stack for this example as follows\. You also can use an existing Windows stack\. Just update the cookbooks, as described later\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and choose **Add Stack**\. Specify the following settings, accept the defaults for the other settings, and choose **Add Stack**\.
   + **Name** – InstallPython
   + **Region** – US West \(Oregon\)

     This example will work in any region, but we recommend using US West \(Oregon\) for tutorials\.
   + **Default operating system** – Microsoft Windows Server 2012 R2

1. Choose **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – Python
   + **Short name** – python

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the Python layer and [start it](workinginstances-starting.md)\.

After the instance is online, you can install the cookbook and run the recipe

**To install the cookbook and run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md), and specify the following settings\.
   + **Repository type** – **S3 Archive**\.
   + **Repository URL** – The cookbook's archive URL that you recorded earlier\.

   Accept the default values for the other settings and choose **Save** to update the stack configuration\.

1. [Run the **Update Custom Cookbooks** stack command](workingstacks-commands.md), which installs the latest version of your custom cookbooks on the stack's online instances\. If an earlier version of your cookbook is present, this command overwrites it\.

1. Execute the recipe by running the **Execute Recipes** stack command with **Recipes to execute** set to **installpython::default**\. This command initiates a Chef run, with a run list that consists of `installpython::default`\.
**Note**  
This example uses **Execute Recipes** for convenience, but you typically have AWS OpsWorks Stacks [run your recipes automatically ](workingcookbook-assigningcustom.md) by assigning them to the appropriate lifecycle event\. You can run such recipes by manually triggering the event\. You can use a stack command to trigger Setup and Configure events, and a [deploy command](workingapps-deploying.md) to trigger Deploy and Undeploy events\.

1. To verify the installation, [use RDP to connect to the instance](workinginstances-rdp.md) and open Windows Explorer\. 
   + The file system should now have a `C:\Python34` directory\.
   + If you run `path` from the command line, it should look something like: `PATH=c:\python34;C:\Windows\system32;...`
   + If you run `python --version` from the command line, it should return `Python 3.4.3`\.