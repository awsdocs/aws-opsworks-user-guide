# Editing AWS OpsWorks Stacks User Settings<a name="opsworks-security-users-manage-edit"></a>

After you have imported users, you can edit their settings, as follows:

**To edit user settings**

1. On the **Users** page, choose **edit** in the user's **Actions** column\.

1. You can specify the following settings\.  
**Self Management**  
Select **Yes** to allow the user to use the MySettings page to specify his or her personal SSH key\.   
You can also enable self\-management by attaching an IAM policy to the user that grants permissions for the [DescribeMyUserProfile](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeMyUserProfile.html) and [UpdateMyUserProfile](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_UpdateMyUserProfile.html) actions\.   
**Public SSH key**  
\(Optional\) Enter a public SSH key for the user\. This key will appear on the user's **My Settings** page\. If you enable self\-management, the user can edit **My Settings** and specify his or her own key\. For more information, see [Registering an IAM User's Public SSH Key](security-settingsshkey.md)\.  
AWS OpsWorks Stacks installs this key on all Linux instances; users can use the associated private key to log in\. For more information, see [Logging In with SSH](workinginstances-ssh.md)\. You cannot use this key with Windows stacks\.  
**Permissions**  
\(Optional\) Set the user's permissions levels for each stack in one place instead of setting them separately by using each stack's **Permissions** page\. For more information on permissions levels, see [Granting Per\-Stack Permissions](opsworks-security-users-console.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions_edit_user.png)