# Importing Users into AWS OpsWorks Stacks<a name="opsworks-security-users-manage-import"></a>

Administrative users can import IAM users into AWS OpsWorks Stacks; they can also import AWS OpsWorks Stacks users from one regional endpoint to another\. When you import IAM users to AWS OpsWorks Stacks, you import them to one of the AWS OpsWorks Stacks regional endpoints\. If you want an IAM user to be available in more than one region, you must import the user to that region\.

While it's not explicitly possible to import federated users in the console, a federated user can implicitly create a user profile by choosing **My Settings** at the upper right of the AWS OpsWorks Stacks console, and then choosing **Users**, also at the upper right\. On the **Users** page, federated users—whose accounts are created by using the API or CLI, or implicitly through the console—can manage their accounts similarly to non\-federated IAM users\.

**To import IAM users into AWS OpsWorks Stacks**

1. Sign in to AWS OpsWorks Stacks as an administrative user or as the account owner\.

1. Choose **Users** at the upper right to open the **Users** page\.  
![\[Users page showing us-east-1 users\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions_users_page.png)

1. Choose **Import IAM Users to <*region name*>** to display the user accounts that are available, but that have not yet been imported\.  
![\[Import commands on Users page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions_import.png)

1. Fill the **Select all** check box, or select one or more individual users\. When you are finished, choose **Import to OpsWorks**\.
**Note**  
After you have imported an IAM user into AWS OpsWorks Stacks, if you use the IAM console or API to delete the user from your account, the user does not automatically lose SSH access that you have granted through AWS OpsWorks Stacks\. You must also delete the user from AWS OpsWorks Stacks by opening the **Users** page, and choosing **delete** in the user's **Actions** column\.

**To import AWS OpsWorks Stacks users from one region to another**

AWS OpsWorks Stacks user accounts are available within the regional endpoint in which they were created\. You can create users in the regions shown in [Users and Regions](opsworks-security-users-manage.md#UsersandRegions)\.

You can import AWS OpsWorks Stacks users from one region to the region to which your **Users** list is currently filtered\. If you import a user to a region that already has a user with the same name, the imported user replaces the existing user\.

1. Sign in to AWS OpsWorks Stacks as an administrative user or as the account owner\.

1. Choose **Users** at the upper right to open the **Users** page\. If you have AWS OpsWorks Stacks users in more than one region, use the **Filter** control to filter for the region to which you want to import users\.  
![\[Users page showing us-east-1 users\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions_users_page.png)

1. Choose **Import AWS OpsWorks Stacks users from another region to <*current region*>**\.  
![\[Users page showing us-west-2 users\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions_import_otherregion.png)

1. Select the region from which you want to import AWS OpsWorks Stacks users\.

1. Select one or more users to import, or select all users, and then choose **Import to this region**\. Wait for AWS OpsWorks Stacks to display the imported users in the **Users** list\.

## Unix IDs and Users Created Outside AWS OpsWorks Stacks<a name="w4ab1c11c59c13c33c15c15"></a>

AWS OpsWorks assigns users on AWS OpsWorks Stacks instances Unix ID \(UID\) values between 2000 and 4000\. Because AWS OpsWorks reserves the 2000\-4000 range of UIDs, users that you create outside of AWS OpsWorks \(by using cookbook recipes, or by importing users into AWS OpsWorks from IAM, for example\) can have UIDs that are overwritten by AWS OpsWorks Stacks for another user\. This can result in users that you have created outside of AWS OpsWorks Stacks not showing up in data bag search results, or being excluded from the AWS OpsWorks Stacks built\-in `sync_remote_users` operation\.

External processes can also create users with UIDs that AWS OpsWorks Stacks can overwrite\. Some operating system packages, for example, can create a user as part of post\-installation processes\. When you or a software process creates a user on a Linux\-based operating system without explicitly specifying a UID—which is the default—the UID assigned by AWS OpsWorks Stacks is *<highest existing AWS OpsWorks UID>* \+ 1\.

As a best practice, create AWS OpsWorks Stacks users and manage their access in the AWS OpsWorks Stacks console, AWS CLI, or by using an AWS SDK\. If you do create users on AWS OpsWorks Stacks instances outside of AWS OpsWorks, use *UnixID* values greater than 4000\.