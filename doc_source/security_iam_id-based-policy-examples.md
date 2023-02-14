# AWS OpsWorks CM Identity\-Based Policy Examples<a name="security_iam_id-based-policy-examples"></a>

By default, users or roles don't have permission to create or modify AWS OpsWorks CM resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant IAM identities permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

In AWS OpsWorks CM, you can assign the `AWSOpsWorksCMServiceRole` policy to a user to let the user create and manage Chef Automate or Puppet Enterprise servers using either the AWS Management Console or AWS CLI\.

**Topics**
+ [Policy Best Practices](#security_iam_policy-best-practices)
+ [Allow Users to View Their Own Permissions](#security_iam_policy-examples-own-permissions)
+ [Viewing AWS OpsWorks CM Servers Based on Tags](#security_iam_policy-examples-view-tags)

## Policy Best Practices<a name="security_iam_policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete OpsWorks CM resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Allow Users to View Their Own Permissions<a name="security_iam_policy-examples-own-permissions"></a>

This example shows how you might create a policy that allows users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "ViewOwnUserInfo",
               "Effect": "Allow",
               "Action": [
                   "iam:GetUserPolicy",
                   "iam:ListGroupsForUser",
                   "iam:ListAttachedUserPolicies",
                   "iam:ListUserPolicies",
                   "iam:GetUser"
               ],
               "Resource": [
                   "arn:aws:iam::*:user/${aws:username}"
               ]
           },
           {
               "Sid": "NavigateInConsole",
               "Effect": "Allow",
               "Action": [
                   "iam:GetGroupPolicy",
                   "iam:GetPolicyVersion",
                   "iam:GetPolicy",
                   "iam:ListAttachedGroupPolicies",
                   "iam:ListGroupPolicies",
                   "iam:ListPolicyVersions",
                   "iam:ListPolicies",
                   "iam:ListUsers"
               ],
               "Resource": "*"
           }
       ]
   }
```

## Viewing AWS OpsWorks CM Servers Based on Tags<a name="security_iam_policy-examples-view-tags"></a>

You can use conditions in your identity\-based policy to control access to AWS OpsWorks CM servers and backups based on tags\. This example shows how you might create a policy that allows viewing a AWS OpsWorks CM server\. However, permission is granted only if the AWS OpsWorks CM server tag `Owner` has the value of that user's user name\. This policy also grants the permissions necessary to complete this action on the console\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListServersInConsole",
            "Effect": "Allow",
            "Action": "opsworks-cm:DescribeServers",
            "Resource": "*"
        },
        {
            "Sid": "ViewServerIfOwner",
            "Effect": "Allow",
            "Action": "opsworks-cm:DescribeServers",
            "Resource": "arn:aws:opsworks-cm:region:master-account-ID:server/server-name",
            "Condition": {
                "StringEquals": {"opsworks-cm:ResourceTag/Owner": "${aws:username}"}
            }
        }
    ]
}
```

You can attach this policy to the users in your account\. If a user named `richard-roe` attempts to view an AWS OpsWorks CM server, the server must be tagged `Owner=richard-roe` or `owner=richard-roe`\. Otherwise he is denied access\. The condition tag key `Owner` matches both `Owner` and `owner` because condition key names are not case\-sensitive\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.