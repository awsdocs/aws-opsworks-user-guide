# Managing AWS OpsWorks Stacks Users<a name="opsworks-security-users-manage"></a>

Before you can import users into AWS OpsWorks Stacks and grant them permissions, you must first have created an IAM user for each individual\. To create IAM users, start by signing in to AWS as an IAM user that has been granted the permissions defined in the IAMFullAccess policy\. You then use the IAM console to [create IAM users](opsworks-security-users-create-user.md) for everyone who needs to access AWS OpsWorks Stacks\. You can then import those users into AWS OpsWorks Stacks and grant user permissions as follows:

**Regular AWS OpsWorks Stacks Users**  
Regular users don't require an attached policy\. If they do have one, it typically does not include any AWS OpsWorks Stacks permissions\. Instead, use the AWS OpsWorks Stacks **Permissions** page to assign one of the following permissions levels to regular users on a stack\-by\-stack basis\.   
+ **Show** permissions allow users to view the stack, but not perform any operations\.
+ **Deploy** permissions include the **Show** permissions and also allow users to deploy and update apps\.
+ **Manage** permissions include the **Deploy** permissions and also allow users to perform stack management operations, such as adding layers or instances, use the **Permissions** page to set user permissions, and enable their own SSH/RDP and sudo/admin privileges\.
+ **Deny** permissions deny access to the stack\.
If these permissions levels are not quite what you want for a particular user, you can customize the user's permissions by attaching an IAM policy\. For example, you might want to use the AWS OpsWorks Stacks **Permissions** page to assign **Manage** permissions level to a user, which grants them permissions to perform all stack management operations, but not to create or clone stacks\. You could then attach a policy that restricts those permissions by denying them permission to add or delete layers or augments those permissions by allowing them to create or clone stacks\. For more information, see [Managing AWS OpsWorks Stacks Permissions by Attaching an IAM PolicyAttaching an IAM Policy](opsworks-security-users-policy.md)\. 

**AWS OpsWorks Stacks Administrative Users**  
Administrative users are the account owner or an IAM user with the permissions that are defined by the [AWSOpsWorksFullAccess policy](opsworks-security-users-examples.md#opsworks-security-users-examples-admin)\. In addition to the permissions granted to **Manage** users, this policy includes permissions for actions that cannot be granted through the **Permissions** page, such as the following:  
+ Importing users into AWS OpsWorks Stacks
+ Creating and cloning stacks
For the complete policy, see [Example Policies](opsworks-security-users-examples.md)\. For a detailed list of permissions that can be granted to users only by attaching an IAM policy, see [AWS OpsWorks Stacks Permissions LevelsPermissions Levels](opsworks-security-users-standard.md)\.

**Topics**
+ [Users and Regions](#UsersandRegions)
+ [Creating an AWS OpsWorks Stacks Administrative User](opsworks-security-users-manage-admin.md)
+ [Creating IAM Users for AWS OpsWorks Stacks](opsworks-security-users-create-user.md)
+ [Importing Users into AWS OpsWorks Stacks](opsworks-security-users-manage-import.md)
+ [Editing AWS OpsWorks Stacks User Settings](opsworks-security-users-manage-edit.md)

## Users and Regions<a name="UsersandRegions"></a>

AWS OpsWorks Stacks user accounts are available within the regional endpoint in which they were created\. You can create users in any of the following regions\.
+ US East \(Ohio\) Region
+ US East \(N\. Virginia\) Region
+ US West \(Oregon\) Region
+ US West \(N\. California\) Region
+ Canada \(Central\) Region \(API only; not available in the AWS Management Console
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

When you import IAM users to AWS OpsWorks Stacks, you import them to one of the regional endpoints; if you want an IAM user to be available in more than one region, you must import the user to that region\. You can also import AWS OpsWorks Stacks users from one region to another; if you import a user to a region that already has a user with the same name, the imported user replaces the existing user\. For more information about importing users, see [Importing Users](opsworks-security-users-manage-import.md)\.