# Managing AWS OpsWorks Stacks User Permissions<a name="opsworks-security-users"></a>

One way to handle AWS OpsWorks Stacks permissions is to attach an IAM AWSOpsWorksFullAccess policy to every IAM user\. However, this policy allows a user to perform every AWS OpsWorks Stacks action on every stack\. It is often desirable instead to restrict AWS OpsWorks Stacks users to a specified set of actions or set of stack resources\. You can control AWS OpsWorks Stacks user permissions in two ways: By using the AWS OpsWorks Stacks **Permissions** page and by attaching an appropriate IAM policy\.

The OpsWorks **Permissions** page—or the equivalent CLI or API actions—allows you to control user permissions in a multiuser environment on a per\-stack basis by assigning each user one of several *permission levels*\. Each level grants permissions for a standard set of actions for a particular stack resource\. Using the **Permissions** page, you can control the following:
+ Who can access each stack\.
+ Which actions each user is allowed to perform on each stack\.

  For example, you can allow some users to only view the stack while others can deploy applications, add instances, and so on\.
+ Who can manage each stack\.

  You can delegate management of each stack to one or more specified users\.
+ Who has user\-level SSH access and sudo privileges \(Linux\) or RDP access and administrator privileges \(Windows\) on each stack's Amazon EC2 instances\.

  You can grant or remove these permissions separately for each user at any time\.

**Important**  
Denying SSH/RDP access does not necessarily prevent a user from logging into instances\. If you specify an Amazon EC2 key pair for an instance, any user with the corresponding private key can log in or use the key to retrieve the Windows administrator password\. For more information, see [Managing SSH Access](security-ssh-access.md)\.

You can use the [IAM console](https://console.aws.amazon.com/iam), CLI, or API to attach policies to your users that grant explicit permissions for the various AWS OpsWorks Stacks resources and actions\.
+ Using an IAM policy to specify permissions is more flexible than using the permissions levels\.
+ You can set up [IAM groups](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html), which grant permissions to groups of users, or define [roles that can be associated with federated users](http://docs.aws.amazon.com/IAM/latest/UserGuide/WorkingWithRoles.html)\.
+ An IAM policy is the only way to grant permissions for certain key AWS OpsWorks Stacks actions\.

  For example, you must use IAM to grant permissions for `opsworks:CreateStack` and `opsworks:CloneStack`, which are used to create and clone stacks, respectively\.

While it's not explicitly possible to import federated users in the console, a federated user can implicitly create a user profile by choosing **My Settings** at the upper right of the AWS OpsWorks Stacks console, and then choosing **Users**, also at the upper right\. On the **Users** page, federated users—whose accounts are created by using the API or CLI, or implicitly through the console—can manage their accounts similarly to non\-federated IAM users\.

The two approaches are not mutually exclusive and it is sometimes useful to combine them; AWS OpsWorks Stacks then evaluates both sets of permissions\. For example, suppose you want to allow users to add or delete instances but not add or delete layers\. None of the AWS OpsWorks Stacks permission levels grant that specific set of permissions\. However, you can use the **Permissions** page to grant users a **Manage** permission level, which allows them to perform most stack operations, and then attach an IAM policy that denies permissions to add or remove layers\. For more information, see [Overview of AWS IAM Permissions](http://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsOverview.html)\. 

The following is a typical model for managing user permissions\. In each case, the reader \(you\) is assumed to be an administrative user\.

1. Use the [IAM console](https://console.aws.amazon.com/iam) to attach AWSOpsWorksFullAccess policies to one or more administrative users\.

1. Create an IAM user for each nonadministrative user with a policy that grants no AWS OpsWorks Stacks permissions\.

   If a user requires access only to AWS OpsWorks Stacks, you might not need to attach a policy at all\. You can instead manage their permissions with the AWS OpsWorks Stacks **Permissions** page\.

1. Use the AWS OpsWorks Stacks **Users** page to import the nonadministrative users into AWS OpsWorks Stacks\.

1. For each stack, use the stack's **Permissions** page to assign a permission level to each user\.

1. As needed, customize users' permission levels by attaching an appropriately configured IAM policy\.

For more recommendations on managing users, see [Best Practices: Managing Permissions](best-practices-permissions.md)\.

**Important**  
As a best practice, don't use root \(account owner\) credentials to perform everyday work in AWS\. Instead, create an IAM administrators group with appropriate permissions\. Then create IAM users for the people in your organization who need to perform administrative tasks \(including for yourself\), and add those users to the administrative group\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html) in the *Using IAM* guide\.

**Topics**
+ [Managing AWS OpsWorks Stacks Users](opsworks-security-users-manage.md)
+ [Granting AWS OpsWorks Stacks Users Per\-Stack Permissions](opsworks-security-users-console.md)
+ [Managing AWS OpsWorks Stacks Permissions by Attaching an IAM Policy](opsworks-security-users-policy.md)
+ [Example Policies](opsworks-security-users-examples.md)
+ [AWS OpsWorks Stacks Permissions Levels](opsworks-security-users-standard.md)