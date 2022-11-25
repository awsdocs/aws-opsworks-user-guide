# Example Policies<a name="opsworks-security-users-examples"></a>

This section describes example IAM policies that can be attached to AWS OpsWorks Stacks users\. 
+ [ Administrative Permissions](#opsworks-security-users-examples-admin) describes policies used to grant permissions to administrative users\.
+ [Manage Permissions](#opsworks-security-users-examples-manage) and [ Deploy Permissions](#opsworks-security-users-examples-deploy) show examples of policies that can be attached to a user to augment or restrict the Manage and Deploy permissions levels\.

  AWS OpsWorks Stacks determines the user's permissions by evaluating the permissions granted by attached IAM policies as well as the permissions granted by the **Permissions** page\. For more information, see [Overview of AWS IAM Permissions](http://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsOverview.html)\. For more information on the **Permissions** page permissions, see [AWS OpsWorks Stacks Permissions LevelsPermissions Levels](opsworks-security-users-standard.md)\.

## Administrative Permissions<a name="opsworks-security-users-examples-admin"></a>

Use the IAM console, [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/), to access the AWSOpsWorks\_FullAccess policy, Attach this policy to a user to grant them permissions to perform all AWS OpsWorks Stacks actions\. The IAM permissions are required, among other things, to allow an administrative user to import users\.

You must create an [IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/WorkingWithRoles.html) that allows AWS OpsWorks Stacks to act on your behalf to access other AWS resources, such as Amazon EC2 instances\. You typically handle this task by having an administrative user create the first stack, and letting AWS OpsWorks Stacks create the role for you\. You can then use that role for all subsequent stacks\. For more information, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\.

The administrative user who creates the first stack must have permissions for some IAM actions that are not included in the AWSOpsWorks\_FullAccess policy\. Add the following permissions to the `Actions` section of the policy\. For proper JSON syntax, be sure to add commas between actions and remove the trailing comma at the end of the list of actions\. 

```
"iam:PutRolePolicy",
"iam:AddRoleToInstanceProfile",
"iam:CreateInstanceProfile",
"iam:CreateRole"
```

## Manage Permissions<a name="opsworks-security-users-examples-manage"></a>

The **Manage** permissions level allows a user to perform a variety of stack management actions, including adding or deleting layers\. This topic describes several policies that you can attach to **Manage** users to augment or restrict the standard permissions\.

Deny a **Manage** user the ability to add or delete layers  
You can restrict the **Manage** permissions level to allow a user perform all **Manage** actions except adding or deleting layers by attaching the following IAM policy\. Replace *region*, *account\_id*, and *stack\_id* with values appropriate to your configuration\.  

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
      "Resource": "arn:aws:opsworks:region:account_id:stack/stack_id/"
    }
  ]
}
```

Allow a **Manage** user to create or clone stacks  
The **Manage** permissions level doesn't allow users to create or clone stacks\. You can change the **Manage** permissions to allow a user to create or clone stacks by attaching the following IAM policy\. Replace *region* and *account\_id* with values appropriate to your configuration\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:GetRolePolicy",
        "iam:ListRoles",
        "iam:ListInstanceProfiles",
        "iam:ListUsers",
        "opsworks:DescribeUserProfiles",
        "opsworks:CreateUserProfile",
        "opsworks:DeleteUserProfile"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "arn:aws:opsworks::account_id:stack/*/",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "opsworks.amazonaws.com"
        }
      }
    }
  ]
}
```

Deny a Manage user the ability to register or deregister resources  
The **Manage** permissions level allows the user to [register and deregister Amazon EBS and Elastic IP address resources](resources-reg.md) with the stack\. You can restrict the **Manage** permissions to allow the user to perform all **Manage** actions except registering resources by attaching the following policy\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "opsworks:RegisterVolume",
        "opsworks:RegisterElasticIp"
      ],
      "Resource": "*"
    }
  ]
}
```

Allow a **Manage** user to import users  
The **Manage** permissions level doesn't allow users to import users into AWS OpsWorks Stacks\. You can augment the **Manage** permissions to allow a user to import and delete users by attaching the following IAM policy\. Replace *region* and *account\_id* with values appropriate to your configuration\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:GetRolePolicy",
        "iam:ListRoles",
        "iam:ListInstanceProfiles",
        "iam:ListUsers",
        "iam:PassRole",
        "opsworks:DescribeUserProfiles",
        "opsworks:CreateUserProfile",
        "opsworks:DeleteUserProfile"
      ],
      "Resource": "arn:aws:iam:region:account_id:user/*",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "opsworks.amazonaws.com"
        }
      }
    }
  ]
}
```

## Deploy Permissions<a name="opsworks-security-users-examples-deploy"></a>

The **Deploy** permissions level doesn't allow users to create or delete apps\. You can augment the **Deploy** permissions to allow a user to create and delete apps by attaching the following IAM policy\. Replace *region*, *account\_id*, and *stack\_id* with values appropriate to your configuration\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "opsworks:CreateApp",
        "opsworks:DeleteApp"
      ],
      "Resource": "arn:aws:opsworks:region:account_id:stack/stack_id/"
    }
  ]
}
```