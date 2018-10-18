# Registering an IAM User's Public SSH Key<a name="security-settingsshkey"></a>

There are two ways to register a user's public SSH key:
+ An administrative user can assign a public SSH key to one or more users and provide them with the corresponding private key\.
+ An administrative user can enable self\-management for one or more users\.

  Those users can then specify their own public SSH key\.

For more information how administrative users can enable self management or assign public keys to users, see [Editing User Settings](opsworks-security-users-manage-edit.md)\.

Connecting to Linux\-based instances by using SSH in a PuTTY terminal requires additional steps\. For more information, see [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) and [Troubleshooting Connecting to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html) in the AWS documentation\.

The following describes how an IAM user with self\-management enabled can specify their public key\.

**To specify your SSH public key**

1. Create an SSH key pair\.

   The simplest approach is to generate the key pair locally\. For more information see [How to Generate Your Own Key and Import It to Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/generating-a-keypair.html#how-to-generate-your-own-key-and-import-it-to-aws)\. 
**Note**  
If you use [PuTTYgen](http://www.putty.org/) to generate your key pair, copy the public key from the **Public key for pasting into OpenSSH authorized\_keys file** box\. Clicking **Save Public Key** saves the public key in a format that is not supported by MindTerm\.

1. Sign into the AWS OpsWorks Stacks console as an IAM user with self\-management enabled\. 
**Important**  
If you sign in as an account owner, or as an IAM user that does not have self\-management enabled, AWS OpsWorks Stacks does not display **My Settings**\. If you are an administrative user or the account owner, you can instead specify SSH keys by going to the **Users** page and [editing the user settings](opsworks-security-users-manage-edit.md)\.

1. Select **My Settings**, which displays the settings for the signed\-in IAM user\.  
![\[My Settings link in OpsWorks dashboard.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions-mysettings-link.png)

1. On the **My Settings** page, click **Edit**\.  
![\[Edit button in My Settings page.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/mysettings-editbutton.png)

1.  In the **Public SSH Key box**, enter your SSH public key, and then click **Save**\.   
![\[Public SSH Key box in My Settings page.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/mysettings-setsshkey.png)

**Important**  
To use the built\-in MindTerm SSH client to connect to Amazon EC2 instances, a user must be signed in as an IAM user and have a public SSH key registered with AWS OpsWorks Stacks\. For more information, see [Using the Built\-in MindTerm SSH Client](workinginstances-ssh-mindterm.md)\.