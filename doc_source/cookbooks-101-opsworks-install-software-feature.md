# Installing a Windows Feature: IIS<a name="cookbooks-101-opsworks-install-software-feature"></a>

 Windows features are a set of optional system components, including the \.NET frameworks and Internet Information Server \(IIS\)\. This topic describes how to implement a cookbook to install a commonly used feature, Internet Information Server \(IIS\)\.

**Note**  
[Installing a Package](cookbooks-101-opsworks-install-software-package.md) shows how to install software that comes in an installer package, such as an MSI file, which you must download to the instance and run\. [IIS cookbooks](https://github.com/opscode-cookbooks/iis) 

[Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md) shows how to use a `powershell_script` resource to install a Windows feature\. This example shows an alternative approach: use the Chef [Windows cookbook's](https://github.com/opscode-cookbooks/windows) `windows_feature` resource\. This cookbook contains a set of resources that use [Deployment Image Servicing and Management](https://technet.microsoft.com/en-us/library/dd744256%28v=ws.10%29.aspx) to perform a variety of tasks on Windows, including feature installation\.

**Note**  
Chef also has an IIS cookbook, which you can use to manage IIS\. For more information, see [IIS cookbook](https://github.com/opscode-cookbooks/iis)\.

**To set up the cookbook**

1. Go to the [windows cookbook GitHub repository](https://github.com/opscode-cookbooks/windows) and download the `windows` cookbook\.

   This example assumes that you will download the `windows` repository as a \.zip file, but you can also clone the repository if you prefer\.

1. Go to the [chef\_handler cookbook GitHub repository](https://github.com/opscode-cookbooks/chef_handler) and download the `chef-handler` cookbook\.

   The `windows` cookbook depends on `chef_handler`; you won't be using it directly\. This example assumes that you will download the `chef_handler` repository as a \.zip file, but you can also clone the repository if you prefer\.

1. Extract the `windows` and `chef_handler` cookbooks to directories in your cookbooks directory named `windows` and `chef_handler`, respectively\.

1. Create a directory in your cookbooks directory named `install-iis` and navigate to it\.

1. Add a `metadata.rb` file to `install-iis` with the following content\.

   ```
   name "install-iis"
   version "0.1.0"
   
   depends "windows"
   ```

   The `depends` directive allows you to use the `windows` cookbook resources in your recipes\.

1. Add a `recipes` directory to `install-iis` and add a file named `default.rb` to that directory that contains the following recipe code\.

   ```
   %w{ IIS-WebServerRole IIS-WebServer }.each do |feature|
     windows_feature feature do
       action :install
     end
   end
   
   service 'w3svc' do
     action [:start, :enable]
   end
   ```

   The recipe uses the `windows` cookbook's `windows_feature` resource to install the following:

   1. The [IIS Web Server role](https://technet.microsoft.com/en-us/library/cc770634.aspx)\.

   1. The [IIS Web Server](https://technet.microsoft.com/en-us/library/cc753433%28v=ws.10%29.aspx)\.

   The recipe then uses a [https://docs.chef.io/chef/resources.html#service](https://docs.chef.io/chef/resources.html#service) resource to start and enable the IIS service \(W3SVC\)\.
**Note**  
For a complete list of available Windows features, [use RDP to log in to the instance](workinginstances-rdp.md), open a command prompt window, and run the following command\. Note that the list is quite long\.  

   ```
   dism /online /Get-Features
   ```

1. Create a `.zip` archive that contains the `install-iis`, `chef_handler`, and `windows` cookbooks and upload the archive to an S3 bucket\. Make the archive public and record the URL for later use\. This example assumes that the archive is named `install-iis.zip`\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

Create a stack for this example as follows\. You also can use an existing Windows stack\. Just update the cookbooks, as described later\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and choose **Add Stack**\. Specify the following settings, accept the defaults for the other settings, and choose **Add Stack**\.
   + **Name** – InstallIIS
   + **Region** – US West \(Oregon\)

     This example will work in any region, but we recommend using US West \(Oregon\) for tutorials\.
   + **Default operating system** – Microsoft Windows Server 2012 R2

1. Choose **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – IIS
   + **Short name** – iis

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the IIS layer and [start it](workinginstances-starting.md)\.

You can now install the cookbook and run the recipe

**To install the cookbook and run the recipe**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md), and specify the following settings\.
   + **Repository type** – **S3 Archive**
   + **Repository URL** – The cookbook archive's URL that you recorded earlier\.

   Accept the default values for the other settings and choose **Save** to update the stack configuration\.

1. [Run the **Update Custom Cookbooks stack** command](workingstacks-commands.md), which installs the latest version of your custom cookbooks on the stack's online instances\. If an earlier version of your cookbooks is present, this command overwrites it\.

1. Execute the recipe by running the **Execute Recipes** stack command with **Recipes to execute** set to **install\-iis::default**\. This command initiates a Chef run, which runs the specified recipes\.
**Note**  
This example uses **Execute Recipes** for convenience, but you typically have AWS OpsWorks Stacks [run your recipes automatically ](workingcookbook-assigningcustom.md) by assigning them to the appropriate lifecycle event\. You can run such recipes by manually triggering the event\. You can use a stack command to trigger Setup and Configure events, and a [deploy command](workingapps-deploying.md) to trigger Deploy and Undeploy events\.

1. To verify the installation, [use RDP to connect to the instance](workinginstances-rdp.md) and open Windows Explorer\. The file system should now have a `C:\inetpub` directory\. If you check the list of services in the Administrative Tools Control Panel application, IIS should be near the bottom\. However, it will be named World Wide Web Publishing Service, not IIS\.