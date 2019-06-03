# Creating an AWS OpsWorks Stacks Administrative User<a name="opsworks-security-users-manage-admin"></a>

You can create an AWS OpsWorks Stacks administrative user by attaching an IAM policy to a user that grants AWS OpsWorks Stacks Full Access permissions\. This procedure assumes that you will create a new IAM user for this purpose\. For existing users, simply add the policy to the user as described in Step 4\.

**To create the IAM User**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. On the [IAM console](https://console.aws.amazon.com/iam/), choose **Users** in the navigation pane, and then choose **Add user**\.

1. Type a user name\. In the **Select AWS access type** area, select **Programmatic access**, and then choose **Next: Permissions**\.

1. On the **Set permissions** page, choose **Attach existing policies directly**\.

1. Enter **OpsWorks** in the **Policy type** filter box to display the AWS OpsWorks policies, select **AWSOpsWorksFullAccess**, and then choose **Next: review**\. This policy grants your user rights to perform all tasks in AWS OpsWorks\.  
![\[\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/iam_user_opsfullaccess.png)

1. On the **Review** page, choose **Create user**\.

1. Choose **Download \.csv**, save the credentials file to a convenient location on your system, and then choose **Close**\.
**Note**  
The AWSOpsWorksFullAccess policy allows users to create and manage AWS OpsWorks Stacks stacks, but users cannot create an IAM service role for the stack; they must use an existing role\. The first user to create a stack must have additional IAM permissions, as described in [ Administrative Permissions](opsworks-security-users-examples.md#opsworks-security-users-examples-admin)\. When this user creates the first stack, AWS OpsWorks Stacks creates an IAM service role with the required permissions\. Thereafter, any user with `opsworks:CreateStack` permissions can use that role to create additional stacks\. For more information, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\.

1. Attach additional customer\-managed policies to fine\-tune the user's permissions, as needed\. For example, you might want an administrative user to be able to create or delete stacks, but not import new users\. For more information, see [Managing AWS OpsWorks Stacks Permissions by Attaching an IAM PolicyAttaching an IAM Policy](opsworks-security-users-policy.md)\.

If you have multiple administrative users, instead of setting permissions separately for each user, you can attach an AWSOpsWorksFullAccess policy to an [IAM group](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html) and add the users to that group\. 
+ To create a new group, choose **Groups** in the navigation pane, and then choose **Create New Group**\. In the **Create New Group Wizard**, name your group and specify the **AWSOpsWorksFullAccess** policy\. You can also specify the **AdministratorAccess** policy, which includes the **AWSOpsWorksFullAccess** permissions\.
+ To add full AWS OpsWorks Stacks permissions to an existing group, choose **Groups** in the navigation pane, select the group, and then choose the **Permissions** tab\. Choose **Attach Policy** or **Attach Another Policy**\. In the **Manage Group Permissions** wizard, select the** AWSOpsWorksFullAccess** policy\.