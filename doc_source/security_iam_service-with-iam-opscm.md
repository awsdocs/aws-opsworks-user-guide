# How AWS OpsWorks CM Works with IAM<a name="security_iam_service-with-iam-opscm"></a>

Before you use IAM to manage access to AWS OpsWorks CM, you should understand what IAM features are available to use with AWS OpsWorks CM\. To get a high\-level view of how AWS OpsWorks CM and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [AWS OpsWorks CM Identity\-Based Policies](#security_iam_service-with-iam-id-based-policies-opscm)
+ [AWS OpsWorks CM and Resource\-Based Policies](#security_iam_resource-based-policies)
+ [Authorization Based on AWS OpsWorks CM Tags](#security_iam_tags)
+ [AWS OpsWorks CM IAM Roles](#security_iam_roles)

## AWS OpsWorks CM Identity\-Based Policies<a name="security_iam_service-with-iam-id-based-policies-opscm"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. AWS OpsWorks CM supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

In AWS OpsWorks CM, you can attach a custom policy statement to an IAM user, role, or group\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions-opscm"></a>

The `Action` element of an IAM identity\-based policy describes the specific action or actions that will be allowed or denied by the policy\. Policy actions usually have the same name as the associated AWS API operation\. The action is used in a policy to grant permissions to perform the associated operation\. 

Policy actions in AWS OpsWorks CM use the following prefix before the action: `opsworks-cm:`\. For example, to grant someone permission to create an AWS OpsWorks CM server by using an API operation, you include the `opsworks-cm:CreateServer` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. AWS OpsWorks CM defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "opsworks-cm:action1",
      "opsworks-cm:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "opsworks-cm:Describe*"
```

When you use wildcards to allow multiple actions in a policy statement, be careful that you are allowing those actions only for authorized services or users\.

To see a list of AWS OpsWorks CM actions, see [Actions, Resources, and Condition Keys for AWS OpsWorks](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsopsworks.html) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources-opscm"></a>

The `Resource` element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. You specify a resource using an ARN or using the wildcard \(\*\) to indicate that the statement applies to all resources\.

You can get the Amazon Resource Number \(ARN\) of an AWS OpsWorks CM server or backup by running the [https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DescribeServers.html](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DescribeServers.html) or [https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DescribeBackups.html](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DescribeBackups.html) API operations, and base resource\-level policies on those resources\.



An AWS OpsWorks CM server resource has an ARN in the following format:

```
arn:aws:opsworks-cm:{Region}:${Account}:server/${ServerName}/${UniqueId}
```

An AWS OpsWorks CM backup resource has an ARN in the following format:

```
arn:aws:opsworks-cm:{Region}:${Account}:backup/${ServerName}-{Date-and-Time-Stamp-of-Backup}
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, to specify the `test-chef-automate` Chef Automate server in your statement, use the following ARN:

```
"Resource": "arn:aws:opsworks-cm:us-west-2:123456789012:server/test-chef-automate/EXAMPLE-d1a2bEXAMPLE"
```

To specify all AWS OpsWorks CM servers that belong to a specific account, use the wildcard \(\*\):

```
"Resource": "arn:aws:opsworks-cm:us-west-2:123456789012:server/*"
```

The following example specifies an AWS OpsWorks CM server backup as a resource:

```
"Resource": "arn:aws:opsworks-cm:us-west-2:123456789012:backup/test-chef-automate-server-2018-05-20T19:06:12.399Z"
```

Some AWS OpsWorks CM actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

Many API actions involve multiple resources\. To specify multiple resources in a single statement, separate the ARNs with commas\.

```
"Resource": [
      "resource1",
      "resource2"
```

To see a list of AWS OpsWorks CM resource types and their ARNs, see [Actions, Resources, and Condition Keys for AWS OpsWorks CM](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsopsworksconfigurationmanagement.html) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions, Resources, and Condition Keys for AWS OpsWorks CM](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsopsworksconfigurationmanagement.html) in the *IAM User Guide*\.

### Condition Keys<a name="security_iam_id-based-policies-conditionkeys"></a>

AWS OpsWorks CM does not have service\-specific context keys that can be used in the `Condition` element of policy statements\. For the list of the global context keys that are available to all services, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#AvailableKeys) in the *IAM Policy Reference*\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can build conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM Policy Elements: Variables and Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\.

### Examples<a name="security_iam-id-based-policies-examples"></a>

To view examples of AWS OpsWorks CM identity\-based policies, see [AWS OpsWorks CM Identity\-Based Policy Examples](security_iam_opscm_id-based-policy-examples.md)\.

## AWS OpsWorks CM and Resource\-Based Policies<a name="security_iam_resource-based-policies"></a>

AWS OpsWorks CM does not support resource\-based policies\.

Resource\-based policies are JSON policy documents that specify what actions a specified principal can perform on a resource and under what conditions\.

## Authorization Based on AWS OpsWorks CM Tags<a name="security_iam_tags"></a>

You can attach tags to AWS OpsWorks CM resources or pass tags in a request to AWS OpsWorks CM\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging AWS OpsWorks CM resources, see [Working with Tags on AWS OpsWorks for Chef Automate Resources](opscm-tags.md) or [Working with Tags on AWS OpsWorks for Puppet Enterprise Resources](opspup-tags.md) in this guide\.

## AWS OpsWorks CM IAM Roles<a name="security_iam_roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

AWS OpsWorks CM uses two roles:
+ A service role that grants the AWS OpsWorks CM service permissions to work within a user's account\. If you use the default service role provided by OpsWorks CM, the name of this role is `aws-opsworks-cm-service-role`\.
+ An instance profile role that lets the AWS OpsWorks CM service call the OpsWorks CM API\. This role grants access to Amazon S3 and AWS CloudFormation to create the server and the S3 bucket for backups\. If you use the default instance profile provided by OpsWorks CM, the name of this instance profile role is `aws-opsworks-cm-ec2-role`\.

AWS OpsWorks CM does not use service\-linked roles\.

### Using Temporary Credentials with AWS OpsWorks CM<a name="security_iam_roles-tempcreds"></a>

AWS OpsWorks CM supports using temporary credentials, and inherits that capability from AWS Security Token Service\.

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\.

### Service\-Linked Roles<a name="security_iam_roles-service-linked"></a>

AWS OpsWorks CM does not use service\-linked roles\.

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

### Service Roles<a name="security_iam_roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

AWS OpsWorks CM uses two roles:
+ A service role that grants the AWS OpsWorks CM service permissions to work within a user's account\. If you use the default service role provided by OpsWorks CM, the name of this role is `aws-opsworks-cm-service-role`\.
+ An instance profile role that lets the AWS OpsWorks CM service call the OpsWorks CM API\. This role grants access to Amazon S3 and AWS CloudFormation to create the server and the S3 bucket for backups\. If you use the default instance profile provided by OpsWorks CM, the name of this instance profile role is `aws-opsworks-cm-ec2-role`\.

### Choosing an IAM Role in AWS OpsWorks CM<a name="security_iam_roles-choose"></a>

When you create a server in AWS OpsWorks CM, you must choose a role to allow AWS OpsWorks CM to access Amazon EC2 on your behalf\. If you have already created a service role, then AWS OpsWorks CM provides you with a list of roles to choose from\. OpsWorks CM can create the role for you, if you do not specify one\. It's important to choose a role that allows access to start and stop Amazon EC2 instances\. For more information, see [Create a Chef Automate Server](gettingstarted-opscm-create.md) or [Create a Puppet Enterprise Master](gettingstarted-opspup-create.md)\.