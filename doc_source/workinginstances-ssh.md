# Using SSH to Log In to a Linux Instance<a name="workinginstances-ssh"></a>

You can log into your online Linux instances with SSH using either the built\-in MindTerm client, or a third\-party client, such as PuTTY\. SSH typically depends on an RSA key pair for authentication\. You install the public key on the instance and provide the corresponding private key to the SSH client\. AWS OpsWorks Stacks handles installing public keys on your stack's instances for you, as follows\.
+ Amazon Elastic Compute Cloud \(Amazon EC2\)key pair – If the stack's region has one or more Amazon EC2 key pairs, you can specify a [default SSH key pair for the stack](workingstacks-creating.md)\.

  You can optionally override the default key pair and specify a different pair when you create an instance\. In either case, AWS OpsWorks Stacks installs the specified key pair's public key on the instance\. For more information on how to create Amazon EC2 key pairs, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.
+ Personal key pair – Each user can [register a personal key pair](security-settingsshkey.md) with AWS OpsWorks Stacks\.

  The user or an administrator registers the public key with AWS OpsWorks Stacks, and the user stores the private key locally\. When setting permissions for a stack, the administrator specifies which users should have SSH access to the stack's instances\. AWS OpsWorks Stacks automatically creates a system user on the stack's instances for each authorized user and installs the users' personal public key\. 

A user must have SSH authorization to use the MindTerm SSH client or to use their personal key pair to log in to a stack's instances\.

**To authorize SSH for an IAM user**

1. In the AWS OpsWorks Stacks navigation pane, click **Permissions**\.

1. Select **SSH/RDP** for the desired AWS Identity and Access Management \(IAM\) user to grant the necessary permissions\. If you want to allow the user to use `sudo` to elevate privileges—for example, to run [agent CLI](agent.md) commands—select **sudo/admin** also\.  
![\[SSH and sudo permissions for users\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions.png)

For more information on how to use AWS OpsWorks Stacks to manage SSH access, see [Managing SSH Access](security-ssh-access.md)\. 

**Topics**
+ [Using the Built\-in MindTerm SSH Client](workinginstances-ssh-mindterm.md)
+ [Using a Third\-Party SSH Client](workinginstances-ssh-third.md)