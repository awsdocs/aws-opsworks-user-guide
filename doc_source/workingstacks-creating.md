# Create a New Stack<a name="workingstacks-creating"></a>

To create a new stack, on the AWS OpsWorks Stacks dashboard, click **Add stack**\. You can then use the **Add Stack** page to configure the stack\. When you are finished, click **Add Stack**\.

**Topics**
+ [Choose the Type of Stack to Create](#workingstacks-creating-decision-table)
+ [Basic Options](#workingstacks-creating-basic)
+ [Advanced Options](#workingstacks-creating-advanced)

## Choose the Type of Stack to Create<a name="workingstacks-creating-decision-table"></a>

Before you create a stack, you must decide the type of stack that you want to create\. For help, see the following table\.


| If you want to create\.\.\. | Create this type of stack if you want to\.\.\. | To learn how, follow these instructions: | 
| --- | --- | --- | 
| A sample stack | Explore the basics of AWS OpsWorks with a Linux\-based Chef 12 stack and a sample Node\.js app\. |  [Getting Started: Sample](gettingstarted-intro.md)   | 
| A Linux\-based Chef 12 stack | Create a Linux\-based stack that uses the latest version of Chef that AWS OpsWorks supports\. Choose this option if you are an advanced Chef user who would like to benefit from the large selection of community cookbooks or write your own custom cookbooks\. For more information, see [Chef 12 Linux](chef-12-linux.md)\. |  [Getting Started: Linux](gettingstarted-linux.md)  | 
| A Windows\-based Chef 12\.2 stack | Create a Windows\-based stack\.  |  [Getting Started: Windows](gettingstarted-windows.md)  | 
| A Linux\-based Chef 11\.10 stack | Create this stack if your organization requires the use of Chef 11\.10 with Linux for backward compatibility\. |  [Getting Started with Chef 11 Linux Stacks](gettingstarted.md)  | 

## Basic Options<a name="workingstacks-creating-basic"></a>

The **Add Stack** page has the following basic options\.

**Stack name**  
\(Required\) A name that is used to identify the stack in the AWS OpsWorks Stacks console\. The name does not need to be unique\. AWS OpsWorks Stacks also generates a stack ID, which is a GUID that uniquely identifies the stack\. For example, with [ AWS CLI](http://aws.amazon.com/documentation/cli/) commands such as [update\-stack](http://docs.aws.amazon.com/cli/latest/reference/opsworks/update-stack.html), you use the stack ID to identify the particular stack\. After you have created a stack, you can find it's ID by choosing **Stack** in the navigation pane and then choosing **Stack Settings**\. The ID is labelled **OpsWorks ID**\.

**Region**  
\(Required\) The AWS region where the instances will be launched\.

**VPC**  
\(Optional\) The ID of the VPC that the stack is to be launched into\. All instances will be launched into this VPC, and you cannot change the ID later\.  
+ If your account supports EC2 Classic, you can specify **No VPC** \(the default value\) if you don't want to use a VPC\.

  For more information about EC2 Classic, see [Supported Platforms](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html)\. 
+ If your account does not support EC2 Classic, you must specify a VPC\.

  The default setting is **Default VPC**, which combines the ease of use of EC2 Classic with the benefits of VPC networking features\. If you want to run your stack in a regular VPC, you must create it by using the VPC [console](https://console.aws.amazon.com/vpc/), [API](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/Welcome.html), or [CLI](http://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/Welcome.html)\. For more information on how to create a VPC for an AWS OpsWorks Stacks stack, see [Running a Stack in a VPC](workingstacks-vpc.md)\. For general information, see [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html)\. 

**Default Availability Zone/Default subnet**  
\(Optional\) This setting depends on whether you are creating your stack in a VPC:  
+ If your account supports EC2 Classic and you set **VPC** to **No VPC**, this setting is labeled **Default Availability Zone**, which specifies the default AWS Availability Zone where the instances will be launched\. 
+ If your account does not support EC2 Classic or you choose to specify a VPC, this field is labeled **Default subnet**, which specifies the default subnet where the instances will be launched\. You can launch an instance in other subnets by overriding this value when you create the instance\. Each subnet is associated with one Availability Zone\.
You can have AWS OpsWorks Stacks launch an instance in a different Availability Zone or subnet by overriding this setting when you [create the instance](workinginstances-add.md)\.  
 For more information about how to run a stack in a VPC, see [Running a Stack in a VPC](workingstacks-vpc.md)\.

**Default operating system**  
\(Optional\) The operating system that is installed by default on each instance\. You have the following options:  
+ One of the built\-in Linux operating systems\.
+ Microsoft Windows Server 2012 R2\.
+ A custom AMI based on one of the supported operating systems\.

  If you select **Use custom AMI**, the operating system is determined by a custom AMI that you specify when you create instances\. For more information, see [Using Custom AMIs](workinginstances-custom-ami.md)\.
For more information on the available operating systems, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.  
You can override the default operating system when you create an instance\. However, you cannot override a Linux operating system to specify Windows, or Windows to specify a Linux operating system\.

**Default SSH key**  
\(Optional\) An Amazon EC2 key pair from the stack's region\. The default value is none\. If you specify a key pair, AWS OpsWorks Stacks installs the public key on the instance\.  
+ With Linux instances, you can use the private key with an SSH client to log in to the stack's instances\.

  For more information, see [Logging In with SSH](workinginstances-ssh.md)\.
+ With Windows instances, you can use the private key with the Amazon EC2 console or CLI to retrieve an instance's Administrator password\.

  You can then use that password with an RDP client to log in to the instance as Administrator\. For more information, see [Logging In with RDP](workinginstances-rdp.md)\.
For more information on how to manage SSH keys, see [Managing SSH Access](security-ssh-access.md)\.  
You can override this setting by specifying a different key pair, or no key pair, when you [create an instance](workinginstances-add.md)\. 

**Chef version**  
This shows the Chef version that you have chosen\.   
For more information on Chef versions, see [Chef Versions](workingcookbook-chef11.md)\.

**Use custom Chef cookbooks**  
Whether to install your custom Chef cookbooks on the stack's instances\.   
For Chef 12, the default setting is **Yes**\. For Chef 11, The default setting is **No**\. The **Yes** option displays several additional settings that provide AWS OpsWorks Stacks with the information it needs to deploy the custom cookbooks from their repository to the stack's instances, such as the repository URL\. The details depend on which repository you use for your cookbooks\. For more information, see [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)\.

**Stack color**  
\(Optional\) The hue used to represent the stack on the AWS OpsWorks Stacks console\. You can use different colors for different stacks to help distinguish, for example, among development, staging, and production stacks\.

**Stack tags**  
You can apply tags at the stack and layer level\. When you create a tag, you are applying the tag to every resource within the tagged structure\. For example, if you apply a tag to a stack, you are applying the tag to every layer, and within each layer, to every instance, Amazon EBS volume, or Elastic Load Balancing load balancer in the layer\. For more information about how to activate your tags and use them to track and manage the costs of your AWS OpsWorks Stacks resources, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) and [Activating User\-Defined Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html) in the *Billing and Cost Management User Guide*\. For more information about tagging in AWS OpsWorks Stacks, see [Tags](tagging.md)\.

## Advanced Options<a name="workingstacks-creating-advanced"></a>

For advanced settings, click **Advanced >>** to display the **Advanced options** and **Security** sections\.

The **Advanced options** section has the following options: 

Default root device type  
Determines the type of storage to be used for the instance's root volume\. For more information, see [Storage](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Storage.html)\.  
+ Linux stacks use an Amazon EBS\-backed root volume by default but you can also specify an instance store\-backed root volume\.
+ Windows stacks must use an Amazon EBS\-backed root volume\. 

IAM role  
\(Optional\) The stack's AWS Identity and Access Management \(IAM\) role, which AWS OpsWorks Stacks uses to interact with AWS on your behalf\. You should use a role that was created by AWS OpsWorks Stacks to ensure that it has all the required permissions\. If you don't have an existing AWS OpsWorks Stacks role, select **New IAM Role** to have AWS OpsWorks Stacks create a new IAM role for you\. For more information, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\.

Default IAM instance profile  
\(Optional\) The default [IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/roles-toplevel.html) to be associated with the stack's Amazon EC2 instances\. This role grants permissions to applications running on the stack's instances to access AWS resources such as S3 buckets\.   
+ To grant specific permissions to applications, choose an existing instance profile \(role\) that has the appropriate policies\.
+ Otherwise, select **New IAM Instance Profile** to have AWS OpsWorks Stacks create a new instance profile for you\.
+ Initially, the profile's role grants no permissions, but you can use the IAM console, API, or CLI to attach appropriate policies\. For more information, see [Specifying Permissions for Apps Running on EC2 instances](opsworks-security-appsrole.md)\.

API endpoint region  
This setting takes its value from the region that you choose in the stack's basic settings\. You can choose from the following regional endpoints\.  
+ US East \(N\. Virginia\) Region
+ US East \(Ohio\) Region
+ US West \(Oregon\) Region
+ US West \(N\. California\) Region
+ Canada \(Central\) Region \(API only; not available for stacks created in the AWS Management Console
+ Asia Pacific \(Mumbai\) Region
+ Asia Pacific \(Singapore\) Region
+ Asia Pacific \(Sydney\) Region
+ Asia Pacific \(Tokyo\) Region
+ Asia Pacific \(Seoul\) Region
+ EU \(Frankfurt\) Region
+ EU \(Ireland\) Region
+ EU \(London\) Region
+ EU \(Paris\) Region
+ South America \(São Paulo\) Region
Stacks that are created in one API endpoint are not available in another API endpoint\. Because AWS OpsWorks Stacks users are also region\-specific, if you want AWS OpsWorks Stacks users in one of these endpoint regions to manage stacks in another endpoint region, you must import the users to the endpoint with which the stacks are associated\. For more information about importing users, see [Importing Users into AWS OpsWorks Stacks](http://docs.aws.amazon.com/opsworks/latest/userguide/opsworks-security-users-manage-import.html)\.

Hostname theme  
\(Optional\) A string that is used to generate a default hostname for each instance\. The default value is **Layer Dependent**, which uses the short name of the instance's layer and appends a unique number to each instance\. For example, the role\-dependent **Load Balancer** theme root is "lb"\. The first instance you add to the layer is named "lb1", the second "lb2", and so on\.

OpsWorks Agent version  <a name="workingstacks-creating-advanced-agent"></a>
\(Optional\) Whether to automatically update the AWS OpsWorks Stacks agent when a new version is available, or use a specified agent version and manually update it\. This feature is available on Chef 11\.10 and Chef 12 stacks\. The default setting is **Manual update**, set to the latest agent version\.  
AWS OpsWorks Stacks installs an agent on each instance that communicates with the service and handles tasks such as initiating Chef runs in response to [lifecycle events](workingcookbook-events.md)\. This agent is regularly updated\. You have two options for specifying the agent version for your stack\.  
+ **Auto\-update** – AWS OpsWorks Stacks automatically installs each new agent version on the stack's instances as soon as the update is available\.
+ **Manual update** – AWS OpsWorks Stacks installs the specified agent version on the stack's instances\.

  AWS OpsWorks Stacks posts a message on the stack page when a new agent version is available, but does not update the stack's instances\. To update the agent, you must manually [update the stack settings](workingstacks-edit.md) to specify a new agent version and AWS OpsWorks Stacks will then update the stack's instances\. 
You can override the default **OpsWorks Agent Version** setting for a particular instance [by updating its configuration](workinginstances-properties.md)\. In that case, the instance's setting takes precedence\. For example, suppose that the default setting is **Auto\-update** but you specify **Manual update** for a particular instance\. When AWS OpsWorks Stacks releases a new agent version, it will automatically update all of the stack's instances except for the one that is set to **Manual update**\. To install a new agent version on that instance, you must manually [update its configuration](workinginstances-properties.md) and specify a new version\.  
The console displays abbreviated agent version numbers\. To see full version numbers, call the AWS CLI [describe\-agent\-versions](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-agent-versions.html) command or the equivalent API or SDK methods\. They return the full version numbers for the available agent versions\.

Custom JSON  
\(Optional\) One or more custom attributes, formatted as a JSON structure\. These attributes are merged into the [stack configuration and deployment attributes](workingcookbook-json.md) that are installed on every instance and can be used by recipes\. You can use custom JSON, for example, to customize configuration settings by overriding the built\-in attributes that specify the default settings\. For more information, see [Using Custom JSON](workingcookbook-json-override.md)\.

**Security** has one option, **Use OpsWorks security groups**, which allows you to specify whether to associate the AWS OpsWorks Stacks built\-in security groups with the stack's layers\.

AWS OpsWorks Stacks provides a standard set of built\-in security groups—one for each layer— which are associated with layers by default\. **Use OpsWorks security groups** allows you to instead provide your own custom security groups\. For more information, see [Using Security Groups](workingsecurity-groups.md)\. 

**Use OpsWorks security groups** has the following settings: 
+ **Yes** \- AWS OpsWorks Stacks automatically associates the appropriate built\-in security group with each layer \(default setting\)\.

  You can associate additional security groups with a layer after you create it but you cannot delete the built\-in security group\.
+ **No** \- AWS OpsWorks Stacks does not associate built\-in security groups with layers\.

  You must create appropriate EC2 security groups and associate a security group with each layer that you create\. However, you can still manually associate a built\-in security group with a layer on creation; custom security groups are required only for those layers that need custom settings\.

Note the following:
+ If **Use OpsWorks security groups** is set to **Yes**, you cannot restrict a default security group's port access settings by adding a more restrictive security group to a layer\. With multiple security groups, Amazon EC2 uses the most permissive settings\. In addition, you cannot create more restrictive settings by modifying the built\-in security group configuration\. When you create a stack, AWS OpsWorks Stacks overwrites the built\-in security groups' configurations with the standard settings, so any changes that you make will be lost the next time you create a stack\. If a layer requires more restrictive security group settings than the built\-in security group, set **Use OpsWorks security groups** to **No**, create custom security groups with your preferred settings, and assign them to the layers on creation\.
+ If you accidentally delete an AWS OpsWorks Stacks security group and want to recreate it, it must an exact duplicate of the original, including the group name's capitalization\. Instead of recreating the group manually, we recommend having AWS OpsWorks Stacks perform the task for you\. Just create a new stack in the same AWS region—and VPC, if present—and AWS OpsWorks Stacks will automatically recreate all the built\-in security groups, including the one that you deleted\. You can then delete the stack if you don't have any further use for it; the security groups will remain\.