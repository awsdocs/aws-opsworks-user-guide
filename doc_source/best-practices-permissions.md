# Best Practices: Managing Permissions<a name="best-practices-permissions"></a>

You must have some form of AWS credentials to access your account's resources\. The following are some general guidelines for providing access to your employees\.
+ First and foremost, we recommend that you do not use your account's root credentials to access AWS resources\.

  Instead, create [IAM users](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html) for your employees and attach policies that provide appropriate access\. Each employee can then use their IAM user credentials to access resources\.
+ Employees should have permissions to access only those resources that they need to perform their jobs\.

  For example, application developers need to access only the stacks that run their applications\. 
+ Employees should have permissions to use only those actions that they need to perform their jobs\.

  An application developer might need full permissions for a development stack and permissions to deploy their apps to the corresponding production stack\. They probably do not need permissions to start or stop instances on the production stack, create or delete layers, and so on\.

For more general information on managing permissions, see [AWS Security Credentials](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)\.

You can use AWS OpsWorks Stacks or IAM to manage user permissions\. Note that the two options are not mutually exclusive; it is sometimes desirable to use both\.

**AWS OpsWorks Stacks Permissions Management**  
Each stack has a **Permissions** page that you can use to grant users permission to access the stack and specify what actions they can take\. You specify a user's permissions by setting one of the following permissions levels\. Each level represents an IAM policy that grants permissions for a standard set of actions\.  
+ **Deny** denies permission to interact with the stack in any way\.
+ **Show** grants permissions view the stack configuration but not modify the stack state in any way\.
+ **Deploy** includes the **Show** permissions and also grants the user permissions to deploy apps\.
+ **Manage** includes the **Deploy** permissions and also allows the user to perform a variety of stack management actions, such as creating or deleting instances and layers\.
The **Manage** permissions level does not grant permissions for a small number of high\-level AWS OpsWorks Stacks actions, including creating or cloning stacks\. You must use an IAM policy to grant those permissions\.
In addition to setting permissions levels, you can also use a stack's **Permissions** page to specify whether users have SSH/RDP and sudo/admin privileges on the stack's instances\. For more information about AWS OpsWorks Stacks permissions management, see [Granting Per\-Stack Permissions](opsworks-security-users-console.md)\. For more information about managing SSH access, see [Managing SSH Access](security-ssh-access.md)\.

**IAM Permissions Management**  
With IAM permissions management, you use the IAM console, API, or CLI to attach a JSON\-formatted policy to a user that explicitly specifies their permissions\. For more information about IAM permissions management, see [What Is IAM?](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html)\.

**Recommendation:** Start with AWS OpsWorks Stacks **Permissions** management\. If you need to fine tune a user's permissions, or grant a user permissions that aren't included in the **Manage** permissions levels, you can combine the two approaches\. AWS OpsWorks Stacks then evaluates both policies to determine the user's permissions\. 

**Important**  
If an IAM user has multiple policies with conflicting permissions, denial always wins\. For example, suppose that you attach an IAM policy to a user that allows access to a particular stack but also use the stack's **Permissions** page to assign the user a **Deny** permissions level\. The **Deny** permissions level takes precedence, and the user will not be able to access the stack\. For more information, see [IAM Policy Evaluation Logic](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_EvaluationLogic.html)\. 

For example, suppose you want a user to be able to perform most operations on a stack, except for adding or deleting layers\.
+ Specify a **Manage** permissions level, which allows the user to perform most stack management actions, including creating and deleting layers\.
+ Attach the following [customer\-managed policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) to the user, which denies permissions to use the [CreateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateLayer.html) and [DeleteLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DeleteLayer.html) actions on that stack\. You identify the stack by its *[Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#ARN)*, which can be found on the stack's **Settings** page\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Deny",
        "Action": [
          "opsworks:CreateLayer",
          "opsworks:DeleteLayer"
        ],
        "Resource": "arn:aws:opsworks:*:*:stack/2f18b4cb-4de5-4429-a149-ff7da9f0d8ee/"
      }
    ]
  }
  ```

For more information, including example policies, see [Managing AWS OpsWorks Stacks Permissions by Attaching an IAM PolicyAttaching an IAM Policy](opsworks-security-users-policy.md)\.

**Note**  
Another way to use IAM policy is to set a condition that limits stack access to employees with a specified IP address or address range\. For example, to ensure that employees access stacks only from inside your corporate firewall, set a condition that limits access to your corporate IP address range\. For more information, see [Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Condition)\.