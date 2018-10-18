# Using RDP to Log In to a Windows Instance<a name="workinginstances-rdp"></a>

You can use the Windows remote desktop protocol \(RDP\) to log in to an online Windows instance, as follows:
+ The instance must have a security group with an inbound rule that allows RDP access\.

  For more information on working with security groups, see [Using Security Groups](workingsecurity-groups.md)\.
+ Ordinary users – AWS OpsWorks Stacks provides authorized ordinary users with an RDP password that is valid for a limited time period, which can range from 30 minutes to 12 hours\.

  In addition to being authorized, users must have at least a [Show permission level](opsworks-security-users-console.md) or their attached AWS Identity and Access Management \(IAM\) policies must allow the `opsworks:GrantAccess` action\.
+ Administrators – You can use the Administrator password to log in for an unlimited amount of time\.

  As described later, if you have specified an Amazon Elastic Compute Cloud \(Amazon EC2\) key pair for the instance, you can use it to retrieve the Administrator password\.

**Note**  
This topic describes how to use the Windows Remote Desktop Connection client to log in from a Windows workstation\. You can also use one of the available RDP clients for Linux or OS X, but the procedure might be somewhat different\. For more information on RDP clients that are compatible with Microsoft Windows Server 2012 R2, see [Microsoft Remote Desktop Clients](https://technet.microsoft.com/en-us/library/dn473009.aspx)\.

**Topics**
+ [Providing a Security Group that Allows RDP Access](#workinginstances-rdp-rdp-ingress)
+ [Logging in As an Ordinary User](#workinginstances-rdp-ordinary)
+ [Logging in As Administrator](#workinginstances-rdp-admin)

## Providing a Security Group that Allows RDP Access<a name="workinginstances-rdp-rdp-ingress"></a>

Before you can use RDP to log into a Windows instance, the instance's security group inbound rules must allow RDP connections\. When you create the first stack in a region, AWS OpsWorks Stacks creates a set of security groups\. They include one named something like `AWS-OpsWorks-RDP-Server`, which AWS OpsWorks Stacks attaches to all Windows instances to allow RDP access\. However, by default, this security group does not have any rules, so you must add an inbound rule to allow RDP access to your instances\.

**To allow RDP access**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/v2/), set it to the stack's region, and choose **Security Groups** from the navigation pane\.

1. Select **AWS\-OpsWorks\-RDP\-Server**, choose the **Inbound** tab, and choose **Edit**\.

1. Choose **Add Rule** and specify the following settings:
   + **Type** – **RDP**
   + **Source** – The permissible source IP addresses\.

     You typically allow inbound RDP requests from your IP address or a specified IP address range \(typically your corporate IP address range\)\.

## Logging in As an Ordinary User<a name="workinginstances-rdp-ordinary"></a>

An authorized user can log in to instances using a temporary password, provided by AWS OpsWorks Stacks\.

**To authorize RDP for an IAM user**

1. In the AWS OpsWorks Stacks navigation pane, click **Permissions**\.

1. Select the **SSH/RDP** checkbox for the desired IAM user to grant the necessary permissions\. If you want the user to have administrator permissions, you should also select **sudo/admin**\.  
![\[SSH and sudo permissions for users\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions.png)

Authorized users can log in to any of the stack's online instances, as follows\.

**To log in as an ordinary user**

1. Log in as an IAM user\.

1. On the **Instances** page, choose **rdp** in the **Actions** column for the appropriate instance\.

1. Specify the session length, which can vary from 30 minutes to 12 hours, and choose **Generate Password**\. The password will be valid only for the specified session duration\.

1. Record the **public DNS name**, **username**, and **password** values, then choose **Acknowledge and close**\.

1. Open the Windows Remote Desktop Connection client, choose **Show Options,** and provide the following from the information that you recorded in Step 4: 
   + **Computer** – The instance's public DNS name\.

     You can also use the public IP address, if you prefer\. Choose **Instances** and copy the address from the instance's **Public IP** column\.
   + **User name** – The user name\.

1. When the client prompts for your credentials, enter the password that you saved in Step 4\.

**Note**  
AWS OpsWorks Stacks generates a user password only for online instances\. If you start an instance and, for example, one of your custom Setup recipes fails, the instance will end up in the `setup_failed` state\. Even though the instance is not online as far as AWS OpsWorks Stacks is concerned, the EC2 instance is running and it's often useful to log in to troubleshoot the issue\. AWS OpsWorks Stacks won't generate a password for you in this case, but if you have assigned an SSH key pair to the instance, you can use the EC2 console or CLI to retrieve the instance's Administrator password and log in as Administrator\. For more information, see the following section\.

## Logging in As Administrator<a name="workinginstances-rdp-admin"></a>

You can log in to an instance as Administrator by using the appropriate password\. If you have assigned an EC2 key pair to an instance, Amazon EC2 uses it to automatically create and encrypt an Administrator password when the instance starts\. You can then use the key pair's private key with the EC2 console, API, or CLI to retrieve and decrypt the password\.

**Note**  
You cannot use a [personal SSH key pair](security-ssh-access.md) to retrieve an Administrator password; you must use an EC2 key pair\. 

The following describes how to use the EC2 console to retrieve an Administrator password and log in to an instance\. If you prefer command\-line tools, you can also use the AWS CLI `[get\-password\-data](http://docs.aws.amazon.com/cli/latest/reference/ec2/get-password-data.html)` command to retrieve the password\.

**To log in as Administrator**

1. Make sure that you have specified an EC2 key pair for the instance\. You can [specify a default key pair for all of the stack's instances](workingstacks-creating.md) when you create the stack, or you can [ specify a key pair for a particular instance](workinginstances-add.md) when you create the instance\.

1. Open the [EC2 console](https://console.aws.amazon.com/ec2/v2/), set it to the stack's region, and choose **Instances** from the navigation pane\.

1. Select the instance, choose **Connect**, and choose **Get Password**\.

1. Provide a path to the EC2 key pair's private key on your workstation, and choose **Decrypt Password**\. Copy the decrypted password for later use\.

1. Open the Windows Remote Desktop Connection client, choose **Show Options,** and provide the following information: 
   + **Computer** – The instance's public DNS name or public IP address, which you can get from the instance's details page\.
   + **User name** – `Administrator`\.

1. When the client prompts for your credentials, provide the decrypted password from Step 4\.