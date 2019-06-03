# Step 2\.3: Implement a Custom Cookbook<a name="gettingstarted-windows-cookbook"></a>

Although a stack is basically a container for instances, you don't add instances directly to a stack\. You add one or more layers, each of which represents a group of related instances, and then add instances to the layers\.

A layer is basically a blueprint that AWS OpsWorks Stacks uses to create a set of Amazon EC2 instances with the same configuration\. An instance starts with a base version of the operating system, and the instance's layer performs a variety of tasks on the instance to implement that blueprint, which can include:
+ Creating directories and files
+ Managing users
+ Installing and configuring software
+ Starting or stopping servers
+ Deploying application code and related files\.

A layer performs tasks on instances by running [Chef recipes](https://docs.chef.io/recipes.html)—recipes for short\. A recipe is a Ruby application that uses Chef's domain\-specific language \(DSL\) to describe the final state of the instance\. With AWS OpsWorks Stacks, each recipe is usually assigned to one of the layer's [lifecycle events](workingcookbook-events.md): Setup, Configuration, Deploy, Undeploy, and Shutdown\. When a lifecycle event occurs on an instance, AWS OpsWorks Stacks runs the event's recipes to perform the appropriate tasks\. For example, the Setup event occurs after an instance finishes booting\. AWS OpsWorks Stacks then runs the Setup recipes, which typically perform tasks such as installing and configuring server software and starting related services\.

AWS OpsWorks Stacks provides each layer with a set of built\-in recipes that perform standard tasks\. You can extend a layer's functionality by implementing custom recipes to perform additional tasks and assigning them to the layer's lifecycle events\. Windows stacks support [custom layers](workinglayers-custom.md), which have a minimal set of recipes that perform only a few basic tasks\. To add functionality to your Windows instances, you must implement custom recipes to install software, deploy applications, and so on\. This topic describes how to create a simple custom layer to support IIS instances\.

**Topics**
+ [A Quick Introduction to Cookbooks and Recipes](#gettingstarted-windows-layer-recipes)
+ [Implement a Recipe to Install and Start IIS](#gettingstarted-windows-layer-recipe-iis)
+ [Enable the Custom Cookbook](#gettingstarted-windows-layer-enable-cookbook)

## A Quick Introduction to Cookbooks and Recipes<a name="gettingstarted-windows-layer-recipes"></a>

A recipe defines one or more aspects of an instance's expected state: what directories it should have, what software packages should be installed, what apps should be deployed, and so on\. Recipes are packaged in a *cookbook*, which typically contains one or more related recipes, plus associated files such as templates for creating configuration files\.

This topic is a very basic introduction to recipes, just enough to show you how to implement a cookbook to support a simple custom IIS layer\. For a more general introduction to cookbooks, see [Cookbooks and Recipes](workingcookbook.md)\. For a detailed tutorial introduction to implementing cookbooks, including some Windows\-specific topics, see [Cookbooks 101](cookbooks-101.md)\.

Chef recipes are technically Ruby applications, but most, if not all, of the code is in the Chef DSL\. The DSL consists largely of a set of *resources*, which you can use to declaratively specify an aspect of the instances state\. For example, a [`directory` resource](https://docs.chef.io/chef/resources.html#directory) defines a directory to be added to the system\. The following example defines a `C:\data` directory with full\-control rights that belongs to the specified user and does not inherit rights from the parent directory\.

```
directory 'C:\data' do
  rights :full_control, 'WORKGROUP\username'
  inherits false
  action :create
end
```

When Chef runs a recipe, it executes each resource by passing the data to an associated *provider*, a Ruby object that handles the details of modifying the instance state\. In this case, the provider creates a new directory with the specified configuration\.

The custom cookbook for the custom IIS layer must perform the following tasks:
+ Install the IIS feature and start the service\.

  You typically perform this task during setup, right after the instance finished booting\.
+ Deploy an app to the instance, a simple HTML page for this example\.

  You typically perform this task during setup\. However, apps usually need to be updated regularly, so you also need to deploy updates while the instance is online\.

You could have a single recipe perform all of these tasks\. However, the preferred approach is to have separate recipes for setup and deployment tasks\. That way, you can deploy app updates at any time without also running setup code\. The following describes how to set up a cookbook to support a custom IIS layer\. Subsequent topics will show how to implement the recipes\.

**To get started**

1. Create a directory named `iis-cookbook` in a convenient location on your workstation\.

1. Add a `metadata.rb` file with the following content to `iis-cookbook`\.

   ```
   name "iis-cookbook"
   version "0.1.0"
   ```

   This example uses a minimal `metadata.rb`\. For more information on how you can use this file, see [metadata\.rb](https://docs.chef.io/config_rb_metadata.html)\.

1. Add a `recipes` directory to `iis-cookbook`\.

   This directory, which must be named `recipes`, contains the cookbook's recipes\.

In general, cookbooks can contain a variety of other directories\. For example, if a recipe uses a template to create a configuration file, the template usually goes in the `templates\default` directory\. The cookbook for this example consists entirely of recipes, so it needs no other directories\. Also, this example uses a single cookbook, but you can use as many as you need; multiple cookbooks are often preferable for complex projects\. For example, you could have separate cookbooks for setup and deployment tasks\. For more cookbook examples, see [Cookbooks and Recipes](workingcookbook.md)\.

## Implement a Recipe to Install and Start IIS<a name="gettingstarted-windows-layer-recipe-iis"></a>

 IIS is a Windows *feature*, one of a set of optional system components that you can install on Windows Server\. You can have a recipe install IIS in either of the following ways:
+ By using a [https://docs.chef.io/chef/resources.html#powershell-script](https://docs.chef.io/chef/resources.html#powershell-script) resource to run the [https://docs.microsoft.com/en-us/powershell/module/servermanager/install-windowsfeature?view=winserver2012-ps](https://docs.microsoft.com/en-us/powershell/module/servermanager/install-windowsfeature?view=winserver2012-ps) cmdlet\.
+ By using the Chef [windows cookbook](https://github.com/opscode-cookbooks/windows) `windows_feature` resource\.

  The `windows` cookbook contains a set of resources whose providers use [Deployment Image Servicing and Management](https://technet.microsoft.com/en-us/library/dd744256%28v=ws.10%29.aspx) \(DISM\) to perform a variety of tasks on Windows instances, including feature installation\.

**Note**  
`powershell_script` is among the most useful resources for Windows recipes\. You can use it to perform a variety of tasks on an instance by running a PowerShell script or cmdlet\. It's especially useful for tasks that aren't supported by a Chef resource\.

This example runs a PowerShell script to install and start Web Server \(IIS\)\. The `windows` cookbook is described later\. For an example of how to use `windows_feature` to install IIS, see [Installing a Windows Feature: IIS](cookbooks-101-opsworks-install-software-feature.md)\.

Add a recipe named `install.rb` with the following contents to the cookbook's `recipes` directory\.

```
powershell_script 'Install IIS' do
  code 'Install-WindowsFeature Web-Server'
  not_if "(Get-WindowsFeature -Name Web-Server).Installed"
end

service 'w3svc' do
  action [:start, :enable]
end
```

The recipe contains two resources\.

**powershell\_script**  
`powershell_script` runs the specified PowerShell script or cmdlet\. The example has the following attribute settings:  
+ `code` – The PowerShell cmdlets to run\.

  This example runs an `Install-WindowsFeature` cmdlet, which installs Web Server \(IIS\)\. In general, the `code` attribute can have any number of lines, so you can run as many cmdlets as you need\.
+ `not-if` – A [https://docs.chef.io/chef/resources.html#guards](https://docs.chef.io/chef/resources.html#guards) that ensures that the recipe installs IIS only if it has not yet been installed\.

  You generally want recipes to be *idempotent*, so they do not waste time performing the same task more than once\.
Every resource has an action, which specifies the action the provider is to take\. There is no explicit action for this example, so the provider takes the default `:run` action, which runs the specified PowerShell script\. For more information, see [Running a Windows PowerShell Script](cookbooks-101-opsworks-opsworks-powershell.md)\.

**service**  
A [https://docs.chef.io/chef/resources.html#service](https://docs.chef.io/chef/resources.html#service) manages a service, the Web Server IIS service \(W3SVC\) in this case\. The example uses default attributes and specifies two actions, `:start` and `:enable`, which start and enable IIS\.

**Note**  
If you want to install software that uses a package installer, such as MSI, you can use a `windows_package` resource\. For more information, see [Installing a Package](cookbooks-101-opsworks-install-software-package.md)\.

## Enable the Custom Cookbook<a name="gettingstarted-windows-layer-enable-cookbook"></a>

AWS OpsWorks Stacks runs recipes from a local cache on each instance\. To run your custom recipes, you must do the following:
+ Store the cookbook in a remote repository\.

  AWS OpsWorks Stacks downloads the cookbooks from this repository to each instance's local cache\.
+ Edit the stack to enable custom cookbooks\.

  Custom cookbooks are disabled by default, so you must enable custom cookbooks for the stack and provide the repository URL and related information\.

AWS OpsWorks Stacks supports S3 archives and Git repositories for custom cookbooks; this example uses an S3 archive\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

**To use an S3 archive**

1. Create a `.zip` archive of the `iis-cookbook` directory\.

   AWS OpsWorks Stacks also supports `.tgz` \(gzip compressed tar\) archives for Windows stacks\.

1. Upload the archive to an S3 bucket in the US West \(N\. California\) region, and make the file public\. You can also use private S3 archives, but public archives are sufficient for this example and somewhat simpler to work with\. 

   1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1. If you do not already have a bucket in `us-west-1`, choose **Create Bucket** and create a bucket in the US West \(N\. California\) region\.

   1. In the buckets list, choose the name of bucket to which you want to upload the file, and then choose **Upload**\. 

   1. Choose **Add Files**\.

   1. Select the archive file to upload, and then choose **Open**\.

   1. At the bottom of the **Upload \- Select Files and Folders** dialog, choose **Set Details**\.

   1. At the bottom of the **Set Details** dialog, choose **Set Permissions**\.

   1. In the **Set Permissions** dialog, choose **Make everything public**\.

   1. At the bottom of the **Set Permissions** dialog, choose **Start Upload**\. When the upload finishes, the `iis-cookbook.zip` file appears in your bucket\.

   1. Choose the bucket, and then choose the **Properties** tab for the bucket\. Next to **Link**, record the archive file's URL for later use\. It should look something like `https://s3-us-west-2.amazonaws.com/windows-cookbooks/iis-cookbook.zip`\.

   For more information about uploading files to an Amazon S3 bucket, see [How Do I Upload Files and Folders to an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html) in the *Amazon S3 Console User Guide*\.

**Important**  
Up to this point, the walkthrough has cost you only a little time; the AWS OpsWorks Stacks service itself is free\. However, you must pay for any AWS resources that you use, such as Amazon S3 storage\. As soon as you upload the archive you begin incurring charges\. For more information, see [AWS Pricing](http://aws.amazon.com/pricing/)\.

**To enable custom cookbooks for the stack**

1. In the AWS OpsWorks Stacks console, choose **Stack** in the navigation pane, and then choose **Stack Settings** on the upper right\.

1. On the upper right of the **Settings** page, choose **Edit**\.

1. On the **Settings** page, set **Use custom Chef cookbooks** to **Yes** and enter the following information:
   + Repository type – **S3 Archive**\.
   + Repository URL – The S3 URL of the cookbook archive file that you recorded earlier\.

1. Choose **Save** to update the stack configuration\.

AWS OpsWorks Stacks installs your custom cookbook on all new instances\. Note that AWS OpsWorks Stacks does not automatically install or update custom cookbooks on online instances\. You can do that manually, as described later\.