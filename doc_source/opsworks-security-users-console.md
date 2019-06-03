# Granting AWS OpsWorks Stacks Users Per\-Stack Permissions<a name="opsworks-security-users-console"></a>

The simplest way to manage AWS OpsWorks Stacks user permissions is by using a stack's **Permissions** page\. Each stack has its own page, which grants permissions for that stack\.

You must be signed in as an administrative user or **Manage** user to modify any of the permissions settings\. The list shows only those users that have been imported into AWS OpsWorks Stacks\. For information on how to create and import users, see [Managing Users](opsworks-security-users-manage.md)\.

The default permission level is IAM Policies Only, which grants users only those permissions that are in their attached IAM policy\.
+ When you import a user from IAM or from another region, the user is added to the list for all existing stacks with an **IAM Policies Only** permission level\.
+ By default, a user whom you have just imported from another region has no access to stacks in the destination region\. If you import users from another region, to let them manage stacks in the destination region, they must be assigned permissions to those stacks after you import the users\.
+ When you create a new stack, all current users are added to the list with **IAM Policies Only** permission levels\.

**Topics**
+ [Setting a User's Permissions](#opsworks-security-users-console-set)
+ [Viewing your Permissions](#opsworks-security-users-console-viewing)
+ [Using IAM Condition Keys to Verify Temporary Credentials](#w4ab1c11c59c13c35c19)

## Setting a User's Permissions<a name="opsworks-security-users-console-set"></a>

**To set a user's permissions**

1. In the navigation pane, choose **Permissions**\.

1. On the **Permissions** page, choose **Edit**\.

1. Change the **Permission level** and **Instance access** settings:
   + Use the **Permissions level** settings to assign one of the standard permission levels to each user, which determine whether the user can access this stack and what actions the user can perform\. If a user has an attached IAM policy, AWS OpsWorks Stacks evaluates both sets of permissions\. For an example see [Example Policies](opsworks-security-users-examples.md)\.
   + The **Instance access** **SSH/RDP** setting specifies whether the user has SSH \(Linux\) or RDP \(Windows\) access to the stack's instances\.

     If you authorize **SSH/RDP** access, you can optionally select **sudo/admin**, which grants the user sudo \(Linux\) or administrative \(Windows\) privileges on the stack's instances\.   
![\[Managing IAM Users with the Permissions page.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions-edit.png)

You can assign each user to one of the following permissions levels\. For a list of the actions that are allowed by each level, see [AWS OpsWorks Stacks Permissions LevelsPermissions Levels](opsworks-security-users-standard.md)\.

**Deny**  
The user cannot perform any AWS OpsWorks Stacks actions on the stack, even if they have an attached IAM policy that grants AWS OpsWorks Stacks full access permissions\. You might use this, for example, to deny some users access to stacks for unreleased products\.

**IAM Policies Only**  
The default level, which is assigned to all newly imported users, and to all users for newly created stacks\. The user's permissions are determined by their attached IAM policy\. If a user has no IAM policy, or their policy has no explicit AWS OpsWorks Stacks permissions, they cannot access the stack\. Administrative users are typically assigned this level because their attached IAM policies already grant full access permissions\.

**Show**  
The user can view a stack, but not perform any operations\. For example, managers might want to monitor an account's stacks, but would not need to deploy apps or modify the stack in any way\.

**Deploy**  
Includes the **Show** permissions and also allows the user to deploy apps\. For example, an app developer might need to deploy updates to the stack's instances but not add layers or instances to the stack\.

**Manage**  
Includes the **Deploy** permissions and also allows the user to perform a variety of stack management operations, including:  
+ Adding or deleting layers and instances\.
+ Using the stack's **Permissions** page to assign permissions levels to users\.
+ Registering or deregistering resources\.
For example, each stack can have a designated manager who is responsible for ensuring that the stack has an appropriate number and type of instances, handling package and operating system updates, and so on\.  
The Manage level does not let users create or clone stacks\. Those permissions must be granted by an attached IAM policy\. For an example, see [ Manage Permissions](opsworks-security-users-examples.md#opsworks-security-users-examples-manage)\.

If the user also has an attached policy, AWS OpsWorks Stacks evaluates both sets of permissions\. This allows you to assign a permission level to a user and then attach a policy to the user to restrict or augment the level's allowed actions\. For example, you could attach a policy that allows a **Manage** user to create or clone stacks, or denies that user the ability to register or deregister resources\. For some examples of such policies, see [Example Policies](opsworks-security-users-examples.md)\.

**Note**  
If the user's policy allows additional actions, the result can appear to override the **Permissions** page settings\. For example, if a user has an attached policy that allows the [CreateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateLayer.html) action but you use the **Permissions** page to specify **Deploy** permissions, the user is still allowed to create layers\. The exception to this rule is the **Deny** option, which denies stack access even to users with AWSOpsWorksFullAccess policies\. For more information, see [Overview of AWS IAM Permissions](http://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsOverview.html)\. 

## Viewing your Permissions<a name="opsworks-security-users-console-viewing"></a>

If [self\-management](opsworks-security-users-manage-edit.md) is enabled, users can see a summary of their permission levels for every stack by choosing **My Settings**, on the upper right\. Users can also access **My Settings** if their attached policy grants permissions for the [DescribeMyUserProfile](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeMyUserProfile.html) and [UpdateMyUserProfile](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_UpdateMyUserProfile.html) actions\.

## Using IAM Condition Keys to Verify Temporary Credentials<a name="w4ab1c11c59c13c35c19"></a>

AWS OpsWorks Stacks has a built\-in authorization layer that supports additional authorization cases \(such as the simplified management of read\-only or read\-write access to stacks for individual users\)\. This authorization layer relies on the usage of temporary credentials\. Because of this, you cannot use an `aws:TokenIssueTime` condition to verify that users are using long\-term credentials, or block actions from users who are using temporary credentials, as described in [Condition Operator to Check Existence of Condition Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Conditions_Null) in the IAM documentation\.