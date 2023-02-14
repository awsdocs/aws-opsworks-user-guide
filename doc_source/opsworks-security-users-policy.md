# Managing AWS OpsWorks Stacks Permissions by Attaching an IAM Policy<a name="opsworks-security-users-policy"></a>

You can specify a user's AWS OpsWorks Stacks permissions by attaching an IAM policy\. An attached policy is required for some permissions:
+ Administrative user permissions, such as importing users\.
+ Permissions for some actions, such as creating or cloning a stack\.

For a complete list of actions that require an attached policy, see [AWS OpsWorks Stacks Permissions LevelsPermissions Levels](opsworks-security-users-standard.md)\. 

You can also use a policy to customize permission levels that were granted through the **Permissions** page\. This section provides a brief summary of how to apply an IAM policy to a user to specify AWS OpsWorks Stacks permissions\. For more information, see [Access management for AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html)\.

An IAM policy is a JSON object that contains one or more *statements*\. Each statement element has a list of permissions, which have three basic elements of their own:

**Action**  
The actions that the permission affects\. You specify AWS OpsWorks Stacks actions as `opsworks:action`\. An `Action` can be set to a specific action such as `opsworks:CreateStack`, which specifies whether the user is allowed to call [http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateStack.html](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateStack.html)\. You can also use wildcards to specify groups of actions\. For example, `opsworks:Create*` specifies all creation actions\. For a complete list of AWS OpsWorks Stacks actions, see the [AWS OpsWorks Stacks API Reference](http://docs.aws.amazon.com/opsworks/latest/APIReference/Welcome.html)\.

**Effect**  
Whether the specified actions are allowed or denied\.

**Resource**  
The AWS resources that the permission affects\. AWS OpsWorks Stacks has one resource type, the stack\. To specify permissions for a particular stack resource, set `Resource` to the stack's ARN, which has the following format: `arn:aws:opsworks:region:account_id:stack/stack_id/`\.  
You can also use wildcards\. For example, setting `Resource` to `*` grants permissions for every resource\. 

For example, the following policy denies the user the ability to stop instances on the stack whose ID is `2860-2f18b4cb-4de5-4429-a149-ff7da9f0d8ee`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "opsworks:StopInstance",
      "Effect": "Deny",
      "Resource": "arn:aws:opsworks:*:*:stack/2f18b4cb-4de5-4429-a149-ff7da9f0d8ee/"
    }
  ]
}
```

For information about adding permissions to an IAM user, see [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console)\.

For more information about how to create or modify IAM policies, see [Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)\. For some examples of AWS OpsWorks Stacks policies, see [Example Policies](opsworks-security-users-examples.md)\.