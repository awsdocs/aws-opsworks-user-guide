# Creating an AWS OpsWorks Stacks Administrative User<a name="opsworks-security-users-manage-admin"></a>

You can create an AWS OpsWorks Stacks administrative user by adding the `AWSOpsWorks_FullAccess` policy to a user, which grants that user AWS OpsWorks Stacks Full Access permissions\. For more information about creating an administrative user, see [Create an administrative user](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-set-up.html#create-an-admin)\.

**Note**  
The AWSOpsWorks\_FullAccess policy allows users to create and manage AWS OpsWorks Stacks stacks, but users cannot create an IAM service role for the stack; they must use an existing role\. The first user to create a stack must have additional IAM permissions, as described in [ Administrative Permissions](opsworks-security-users-examples.md#opsworks-security-users-examples-admin)\. When this user creates the first stack, AWS OpsWorks Stacks creates an IAM service role with the required permissions\. Thereafter, any user with `opsworks:CreateStack` permissions can use that role to create additional stacks\. For more information, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\.

When you create a user, you can add additional customer\-managed policies to fine\-tune the user's permissions, as needed\. For example, you might want an administrative user to be able to create or delete stacks, but not import new users\. For more information, see [Managing AWS OpsWorks Stacks Permissions by Attaching an IAM PolicyAttaching an IAM Policy](opsworks-security-users-policy.md)\.

If you have multiple administrative users, instead of setting permissions separately for each user, you can add the AWSOpsWorks\_FullAccess policy to an IAM group and add the users to that group\. 

For information about creating a group, see [Creating IAM user groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_create.html)\. When you create the group, add the **AWSOpsWorks\_FullAccess** policy\. You can also add the **AdministratorAccess** policy, which includes the **AWSOpsWorks\_FullAccess** permissions\.

For information about adding permissions to an existing group, see [Attaching a policy to an IAM user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html)\.