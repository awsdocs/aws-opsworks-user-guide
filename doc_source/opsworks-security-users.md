# Managing AWS OpsWorks Stacks User Permissions<a name="opsworks-security-users"></a>

As a best practice, restrict AWS OpsWorks Stacks users to a specified set of actions or set of stack resources\. You can control AWS OpsWorks Stacks user permissions in two ways: by using the AWS OpsWorks Stacks **Permissions** page, and by applying an appropriate IAM policy\.

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

You can use the [IAM console](https://console.aws.amazon.com/iam), CLI, or API to add policies to your users that grant explicit permissions for the various AWS OpsWorks Stacks resources and actions\.
+ Using an IAM policy to specify permissions is more flexible than using the permissions levels\.
+ You can set up [IAM Identities \(users, user groups, and roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html), which grant permissions to IAM identities, such as users and user groups, or define [roles ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that can be associated with federated users\.
+ An IAM policy is the only way to grant permissions for certain key AWS OpsWorks Stacks actions\.

  For example, you must use IAM to grant permissions for `opsworks:CreateStack` and `opsworks:CloneStack`, which are used to create and clone stacks, respectively\.

While it's not explicitly possible to import federated users in the console, a federated user can implicitly create a user profile by choosing **My Settings** at the upper right of the AWS OpsWorks Stacks console, and then choosing **Users**, also at the upper right\. On the **Users** page, federated users—whose accounts are created by using the API or CLI, or implicitly through the console—can manage their accounts similarly to non\-federated users\.

The two approaches are not mutually exclusive and it is sometimes useful to combine them; AWS OpsWorks Stacks then evaluates both sets of permissions\. For example, suppose you want to allow users to add or delete instances but not add or delete layers\. None of the AWS OpsWorks Stacks permission levels grant that specific set of permissions\. However, you can use the **Permissions** page to grant users a **Manage** permission level, which allows them to perform most stack operations, and then apply an IAM policy that denies permissions to add or remove layers\. For more information, see [Controlling access to AWS resources using policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_controlling.html)\. 

The following is a typical model for managing user permissions\. In each case, the reader \(you\) is assumed to be an administrative user\.

1. Use the [IAM console](https://console.aws.amazon.com/iam) to apply AWSOpsWorks\_FullAccess policies to one or more administrative users\.

1. Create a user for each nonadministrative user with a policy that grants no AWS OpsWorks Stacks permissions\.

   If a user requires access only to AWS OpsWorks Stacks, you might not need to apply a policy at all\. You can instead manage their permissions with the AWS OpsWorks Stacks **Permissions** page\.

1. Use the AWS OpsWorks Stacks **Users** page to import the nonadministrative users into AWS OpsWorks Stacks\.

1. For each stack, use the stack's **Permissions** page to assign a permission level to each user\.

1. As needed, customize users' permission levels by applying an appropriately configured IAM policy\.

For more recommendations about managing users, see [Best Practices: Managing Permissions](best-practices-permissions.md)\.

For more information about IAM best practices, see [ Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

**Topics**
+ [Managing AWS OpsWorks Stacks Users](opsworks-security-users-manage.md)
+ [Granting AWS OpsWorks Stacks Users Per\-Stack Permissions](opsworks-security-users-console.md)
+ [Managing AWS OpsWorks Stacks Permissions by Attaching an IAM Policy](opsworks-security-users-policy.md)
+ [Example Policies](opsworks-security-users-examples.md)
+ [AWS OpsWorks Stacks Permissions Levels](opsworks-security-users-standard.md)