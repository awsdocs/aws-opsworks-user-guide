# Adding an Instance to a Layer<a name="workinginstances-add"></a>

After you create a layer, you usually add at least one instance\. You can add more instances later, if the current set can't handle the load\. You can also use [load\-based or time\-based instances](workinginstances-autoscaling.md) to automatically scale the number of instances\.

You can add either new or existing instances to a layer:
+ **New**–OpsWorks creates a new instance, configured to your specifications, and makes it a member of the layer\.
+ **Existing**–You can add an existing instance from any compatible layer, but it must be in the offline \(stopped\) state\.

If an instance belongs to multiple layers, AWS OpsWorks Stacks runs the recipes for each of the instance's layers when a lifecycle event occurs, or when you run a [stack](workingstacks-commands.md) or [deployment](workingapps-deploying.md) command\.

You can also make an instance a member of multiple layers by editing its configuration\. For more information, see [Editing the Instance Configuration](workinginstances-properties.md)\.

**Note**  
[Spot instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/how-spot-instances-work.html) are not supported\.

**To add a new instance to a layer**

1. On the **Instances** page, choose **\+Instance** for the appropriate layer and \(if necessary\) choose the **New** tab\. If you want to configure more than just the **Host name**, **Size**, and **Subnet** or **Availability Zone**, choose **Advanced >>** to see more options\. The following shows the complete set of options:  
![\[+Instance for new instances on Instances page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add_instance.png)

1. If desired, you can override the default configuration, most of which you specified when you created the stack\. For more information, see [Create a New Stack](workingstacks-creating.md)\.  
**Hostname**  
Identifies the instance on the network\. By default, AWS OpsWorks Stacks generates each instance's host name by using the **Hostname theme** you specified when you created the stack\. You can override this value and specify your preferred host name\.  
**Size**  
An Amazon EC2 instance type, which specifies the instance's resources, such as the amount of memory or number of virtual cores\. AWS OpsWorks Stacks specifies a default size for each instance, which you can override with your preferred instance type\.   
The instance types supported by AWS OpsWorks Stacks depend on whether or not the stack is in a VPC\. Instance types are also limited if your account is using the AWS Free Tier\. The drop\-down **Size** list shows the supported instance types for the Chef version that your stack supports\. Be aware that micro instances such as t1\.micro might not have sufficient resources to support some layers\. For more information, see [Instance Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html)\.   
If you are using [load\-balanced instances](workinginstances-autoscaling-loadbased.md), note that [Configure lifecycle events](workingcookbook-events.md) can produce a significant CPU load spike that might last a minute or longer\. With smaller instances this load spike can be enough to trigger upscaling, especially for large load\-balanced stacks with frequent Configure events\. The following are some ways to reduce the likelihood of a Configure event causing needless upscaling\.  
   + Use larger instances, so that the additional load from a Configure event is not enough to trigger upscaling\.
   + Don't use instance types such as T2 that share CPU resources\. 

     This ensures that when a Configure event occurs, all of the instance's CPU resources are immediately available\.
   + Make the `exceeded threshold` time significantly longer than the time required to process a Configure event, perhaps 5 minutes\.

     For more information, see [Using Automatic Load\-based Scaling](workinginstances-autoscaling-loadbased.md)\.  
**Availability Zone/Subnet**  
If the stack is not in a VPC, this setting is labeled ** Availability Zone** and lists the region's zones\. You can use this setting to override the default Availability Zone you specified when you created the stack\.  
If the stack is running in a VPC, this setting is labeled **Subnet** and lists the VPC's subnets\. You can use this setting to override the default subnet you specified when you created the stack\.  
By default, AWS OpsWorks Stacks lists the subnet's CIDR ranges\. To make the list more readable, use the VPC console or API to add a tag to each subnet with **Key** set to **Name** and **Value** set to the subnet's name\. AWS OpsWorks Stacks appends that name to the CIDR range\. In the preceding example, the subnet's Name tag is set to **Private**\.  
**Scaling Type**  
Determines how the instance is started and stopped\.   
   + The default value is a **24/7** instance, which you start and stop manually\.
   + AWS OpsWorks Stacks starts and stops **time\-based** instances based on a specified schedule\.
   + \(Linux only\) AWS OpsWorks Stacks starts and stops **load\-based** instances based on specified load metrics\.
You do not start or stop load\-based or time\-based instances yourself\. Instead, you configure the instances, and AWS OpsWorks Stacks starts and stops them based on the configuration\. For more information, see [Managing Load with Time\-based and Load\-based Instances](workinginstances-autoscaling.md)\.  
**SSH key**  
An Amazon EC2 key pair\. AWS OpsWorks Stacks installs the public key on the instance\.  
   + For Linux instances, you can use the corresponding private key with an SSH client to [log in to the instance](workinginstances-ssh.md)\. 
   + For Windows instances, you can use the corresponding private key to [retrieve the instance's Administrator password](workinginstances-rdp.md#workinginstances-rdp-admin)\. You can then use that password with RDP to log into the instance as Administrator\.
Initially, this setting is the **Default SSH key** value that you specified when you created the stack\.  
   + If the default value is set to **Do not use a default SSH key**, you can specify one of your account's Amazon EC2 keys\.
   + If the default value is set to an Amazon EC2 key, you can specify a different key or no key\.  
**Operating system**  
**Operating system** specifies which operating system the instance is running\. AWS OpsWorks Stacks supports only 64\-bit operating systems\.  
 Initially, this setting is the **Default operating system** value that you specified when you created the stack\. You can override the default value to specify a different Linux operating system or a custom Amazon Machine Image \(AMI\)\. However, you cannot switch from Linux to Windows or from Windows to Linux\.  
If you select **Use custom AMI**, the page displays a list of custom AMIs instead of **Architecture** and **Root device type**\.  

![\[+Instance for new instances on Instances page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add_instance_ami.png)
For more information, see [Using Custom AMIs](workinginstances-custom-ami.md)\.  
**OpsWorks Agent version**  
**OpsWorks Agent version** specifies the version of the AWS OpsWorks Stacks agent that you want to run on the instance\. If you want AWS OpsWorks Stacks to update the agent automatically, choose **Inherit from stack**\. To install a specific version of the agent, and manually update the agent on the instance, choose a version from the drop\-down list\.  
Not all agent versions work with all operating system releases\. If your instance is running an agent–or you install an agent on an instance–that is not fully supported on the instance operating system, the AWS OpsWorks Stacks console displays error messages that instruct you to install a compatible agent\.  
**Tenancy**  
Choose the tenancy option for your instance\. You can choose to run your instances on physical servers fully dedicated for your use\.  
   + **Default \- Rely on VPC settings**\. No tenancy, or inherits tenancy settings from your VPC\.
   + **Dedicated \- Run a dedicated instance**\. Pay by the hour for instances that run on single\-tenant hardware\. For more information, see [Dedicated Instances](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/dedicated-instance.html) in the Amazon VPC User Guide, and [Amazon EC2 Dedicated Instances](https://aws.amazon.com/ec2/purchasing-options/dedicated-instances/)\.
   + **Dedicated host \- Run this instance on a dedicated host**\. Pay for a physical host that is fully dedicated to running your instances, and bring your existing per\-socket, per\-core, or per\-VM software licenses to reduce costs\. For more information, see [Dedicated Hosts Overview](https://aws.amazon.com/ec2/dedicated-hosts/) in the Amazon EC2 documentation, and [Amazon EC2 Dedicated Hosts](https://aws.amazon.com/ec2/dedicated-hosts/)\.  
**Root device type**  
Specifies the instance's root device storage\.  
   + Linux instances can be either Amazon EBS\-backed or instance store\-backed\.
   + Windows instances must be Amazon EBS\-backed\.
For more information, see [Storage](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Storage.html)\.  
After the initial boot, Amazon EBS\-backed instances boot faster than instance store\-backed instances because AWS OpsWorks Stacks does not have to reinstall the instance's software from scratch\. For more information, see [Root Device Storage](best-practices-storage.md)\.  
**Volume type**  
Specifies the root device volume type: **Magnetic**, **Provisioned IOPS \(SSD\)**, or **General Purpose \(SSD\)**\. For more information, see [Amazon EBS Volume Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)\.  
**Volume size**  
Specifies the root device volume size for the specified volume type\. For more information, see [Amazon EBS Volume Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)\.  
   + **General Purpose \(SSD\)**\. Minimum allowed size is: 8 GiB; maximum size is 16384 GiB\.
   + **Provisioned IOPS \(SSD\)**\. Minimum allowed size is: 8 GiB; maximum size is 16384 GiB\. You can set a minimum of 100 input/output operations per second \(IOPS\), and a maximum of 240 IOPS\.
   + **Magnetic**\. Minimum allowed size is 8 GiB; maximum size is 1024 GiB\.

1. Choose **Add Instance** to create the new instance\.

**Note**  
You cannot override the [stack's default agent version](workingstacks-creating.md#workingstacks-creating-advanced-agent) setting when you create an instance\. To specify a custom agent version setting, you must create the instance and then [edit its configuration](workinginstances-properties.md)\.

**To add an existing instance to a layer**

1. On the **Instances** page, choose **\+Instance** for the appropriate layer, and then open the **Existing** tab\. 
**Note**  
If you change your mind about using an existing instance, choose **New** to create a new instance as described in the preceding procedure\.

1. On the **Existing** tab, select an instance from the list\.

1. Choose **Add Instance** to create the new instance\.

An instance represents an Amazon EC2 instance, but is basically just an AWS OpsWorks Stacks data structure\. An instance must be started to create a running Amazon EC2 instance, as described in the following sections\.

**Important**  
If you launch instances into a default VPC, you must be careful about modifying the VPC configuration\. The instances must always be able to communicate with the AWS OpsWorks Stacks service, Amazon S3, and package repositories\. If, for example, you remove a default gateway, the instances will lose their connection to the AWS OpsWorks Stacks service, which will then treat the instances as failed and [auto heal](workinginstances-autohealing.md) them\. However, AWS OpsWorks Stacks will not be able to install the instance agent on the healed instances\. Without an agent, the instances cannot communicate with the service, and the startup process will not progress beyond the `booting` status\. For more information on default VPC, see [Supported Platforms](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html)\.

You can also incorporate Linux computing resources into a stack that were created outside of AWS OpsWorks Stacks:
+ Amazon EC2 instances that you created directly by using the Amazon EC2 console, CLI, or API\.
+ *On\-premises* instances running on your own hardware, including instances running in virtual machines\.

For more information, see [Using Computing Resources Created Outside of AWS OpsWorks Stacks](registered-instances.md)\.