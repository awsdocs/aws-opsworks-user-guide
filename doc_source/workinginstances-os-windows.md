# Microsoft Windows Server<a name="workinginstances-os-windows"></a>

The following notes describe AWS OpsWorks Stacks support for Windows instances\. Windows instances are available only for Chef 12\.2 stacks\. The exact version of Chef in a Windows stack is 12\.22\.

Currently, the AWS OpsWorks Stacks agent cannot be installed on—and AWS OpsWorks Stacks cannot manage—Windows\-based instances that use a system UI language other than **English \- United States** \(en\-US\)\.

**Versions**  
AWS OpsWorks Stacks supports the following 64\-bit versions of [Microsoft Windows Server 2012 R2](http://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx):  
+ Microsoft Windows Server 2012 R2 Standard
+ Microsoft Windows Server 2012 R2 with SQL Server Express
+ Microsoft Windows Server 2012 R2 with SQL Server Standard
+ Microsoft Windows Server 2012 R2 with SQL Server Web
Amazon Elastic Compute Cloud \(Amazon EC2\) refers to Microsoft Windows Server 2012 R2 Standard as Microsoft Windows Server 2012 R2 *Base*, which is what you will see reported as the operating system on Windows instances\. You can also use a custom AMI based on these operating systems\. For pricing information, go to the [Amazon EC2 Pricing](https://aws.amazon.com/ec2/pricing/) page and visit the Windows tabs under **On\-Demand Instances**\. 

**Creating Instances**  
You create Windows instances with the AWS OpsWorks Stacks console, API, or CLI\. Windows instances are Amazon EBS\-backed, but you cannot mount extra Amazon EBS volumes\.  
Windows stacks can use [24/7](workinginstances-starting.md) instances, which you start and stop manually\. They can also use [time\-based automatic scaling](workinginstances-autoscaling-timebased.md), which automatically starts and stops instances based on a user\-specified schedule\. Windows\-based stacks cannot use [load\-based automatic scaling](workinginstances-autoscaling-loadbased.md)\.  
You cannot [register Windows instances](registered-instances.md) that were created outside of AWS OpsWorks Stacks with a stack\.

**Updates**  
AWS updates Windows AMIs for each set of patches, so when you create an instance, it will have the latest updates\. However, AWS OpsWorks Stacks does not provide a way to apply updates to online Windows instances\. The simplest way to ensure that Windows is up to date is to replace your instances regularly, so that they are always running the latest AMI\.

**Layers**  
To handle tasks such as installing and configuring software or deploying apps, you will need to implement one or more [custom layers](workinglayers-custom.md) with custom recipes\.

**Chef**  
Windows instances use Chef 12\.22, and run [chef\-client in local mode](https://docs.chef.io/ctl_chef_client.html#run-in-local-mode), which launches a local in\-memory Chef server called [chef\-zero](https://docs.chef.io/ctl_chef_client.html#about-chef-zero)\. The presence of this server enables custom recipes to use Chef search and data bags\.

**Remote Login**  
AWS OpsWorks Stacks provides authorized AWS Identity and Access Management \(IAM\) users with a password that they can use to log in to Windows instances\. This password expires after a specified time\. Administrators can use an SSH key pair to retrieve an instance's Administrator password, which provides unlimited [RDP access](workinginstances-rdp.md)\. For more information, see [Logging In with RDP](workinginstances-rdp.md)\.

**AWS SDK**  
AWS OpsWorks Stacks automatically installs the [AWS SDK for \.NET](http://aws.amazon.com/sdk-for-net/) on each instance\. This package includes the AWS \.NET libraries and AWS Tools for Windows, including the [AWS Tools for PowerShell](http://aws.amazon.com/powershell/)\. To use the Ruby SDK, you can have a custom recipe install the appropriate gem\.

**Monitoring and Metrics**  
Windows instances support the standard [Amazon CloudWatch \(CloudWatch\) metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatch.html), which you can view in the CloudWatch console\.

**Ruby**  
The Chef 12\.22 client that AWS OpsWorks Stacks installs on Windows instances comes with Ruby 2\.3\.6\. However, AWS OpsWorks Stacks does not add the executable's directory to the PATH environment variable\. To have your applications use this Ruby version, you can typically find it in `C:\opscode\chef\embedded\bin\`\.

**AWS OpsWorks Stacks Agent CLI**  
The AWS OpsWorks Stacks agent on Windows instances does not expose a [command\-line interface](agent.md)\.

**Proxy Support**  
Do the following to set up proxy support for Windows instances:  

1. Modify `machine.config` to add the following, which adds proxy support to Windows PowerShell \(initial bootstrap\) and \.NET \(AWS OpsWorks Stacks agent\) applications:

   ```
   <system.net>
     <defaultProxy>
       <proxy autoDetect="false" bypassonlocal="true" proxyaddress="http://10.100.1.91:3128"  usesystemdefault="false" />
       <bypasslist>
         <add address="localhost" />
         <add address="169.254.169.254" />
       </bypasslist>
     </defaultProxy>
   </system.net>
   ```

1. Run the following commands to set environment variables for later use by Chef and Git:

   ```
   setx /m no_proxy "localhost,169.254.169.254"
   setx /m http_proxy "http://10.100.1.91:3128"
   setx /m https_proxy "http://10.100.1.91:3128"
   ```

**Note**  
For more control over how AWS OpsWorks Stacks updates your instances, create a custom AMI based on Microsoft Windows Server 2012 R2 Base\. For example, with custom AMIs you can specify which software is installed on an instance, such as Web Server \(IIS\)\. For more information, see [Using Custom AMIs](workinginstances-custom-ami.md)\.