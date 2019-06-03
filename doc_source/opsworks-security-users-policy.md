# Managing AWS OpsWorks Stacks Permissions by Attaching an IAM Policy<a name="opsworks-security-users-policy"></a>

You can specify a user's AWS OpsWorks Stacks permissions by attaching an IAM policy\. An attached policy is required for some permissions:
+ Administrative user permissions, such as importing users\.
+ Permissions for some actions, such as creating or cloning a stack\.

For a complete list of actions that require an attached policy, see [AWS OpsWorks Stacks Permissions LevelsPermissions Levels](opsworks-security-users-standard.md)\. 

You can also use an attached policy to customize permission levels that were granted through the **Permissions** page\. This section provides a brief summary of how to attach an IAM policy to a user to specify AWS OpsWorks Stacks permissions\. For more information, see [Permissions and Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html)\.

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

**To attach a policy to an IAM user**

1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. To create a new managed policy for this user, choose **Policies** in the navigation pane, and then in the list of policies, choose **Create Policy** to create a new customer\-managed policy with the appropriate permissions\. For more information about how to create a customer\-managed policy, see [Customer Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#customer-managed-policies)\.

1. When you have created the customer\-managed policy, choose **Users** in the IAM console navigation pane, select the user name, and then choose **Add permissions**\.

1. On the **Add permissions** page, choose **Attach existing policies directly**\.

1. In the **Policy Type** drop\-down list, select **Customer managed**, select the policy that you want to attach, and then choose **Next: Review**\.

1. On the **Review** page, choose **Add permissions**\.

For more information about how to create or modify IAM policies, see [Permissions and Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html)\. For some examples of AWS OpsWorks Stacks policies, see [Example Policies](opsworks-security-users-examples.md)\.