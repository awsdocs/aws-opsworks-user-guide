# Cross\-service confused deputy prevention in AWS OpsWorks CM<a name="cross-service-confused-deputy-prevention"></a>

The confused deputy problem is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. In AWS, cross\-service impersonation can result in the confused deputy problem\. Cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources in a way it should not otherwise have permission to access\. To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

We recommend using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) global condition context keys in resource policies to limit the permissions that AWS OpsWorks CM gives another service to the resource\. If the `aws:SourceArn` value does not contain the account ID, such as an Amazon S3 bucket ARN, you must use both global condition context keys to limit permissions\. If you use both global condition context keys and the `aws:SourceArn` value contains the account ID, the `aws:SourceAccount` value and the account in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\. Use `aws:SourceArn` if you want only one resource to be associated with the cross\-service access\. Use `aws:SourceAccount` if you want to allow any resource in that account to be associated with the cross\-service use\.

The value of `aws:SourceArn` must be the ARN of an OpsWorks CM Chef or Puppet server\.

The most effective way to protect against the confused deputy problem is to use the `aws:SourceArn` global condition context key with the full ARN of the AWS OpsWorks CM server\. If you don't know the full ARN, or if you are specifying multiple server ARNs, use the `aws:SourceArn` global context condition key with wildcards \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:servicename:*:123456789012:*`\.

The following section shows how you can use the `aws:SourceArn` and `aws:SourceAccount` global condition context keys in AWS OpsWorks CM to prevent the confused deputy problem\.

## Prevent confused deputy exploits in AWS OpsWorks CM<a name="confused-deputy-opsworks-cm-procedure"></a>

This section describes how you can help prevent confused deputy exploits in AWS OpsWorks CM, and includes examples of permissions policies that you can attach to the IAM role you are using to access AWS OpsWorks CM\. As a security best practice, we recommend adding the `aws:SourceArn` and `aws:SourceAccount` condition keys to the trust relationships your IAM role has with other services\. The trust relationships allow AWS OpsWorks CM to assume a role to perform actions in other services that are required to create or manage your AWS OpsWorks CM servers\.

**To edit trust relationships to add `aws:SourceArn` and `aws:SourceAccount` condition keys**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles**\.

1. In the **Search** box, search for the role that you use for access to AWS OpsWorks CM\. The AWS managed role is `aws-opsworks-cm-service-role`\.

1. On the **Summary** page for the role, choose the **Trust relationships** tab\.

1. On the **Trust relationships** tab, choose **Edit trust relationship**\.

1. In the **Policy document**, add at least one of the `aws:SourceArn` or `aws:SourceAccount` condition keys to the policy\. Use `aws:SourceArn` to restrict the trust relationship between cross services \(such as AWS Certificate Manager and Amazon EC2\) and AWS OpsWorks CM to specific AWS OpsWorks CM servers, which is more restrictive\. Add `aws:SourceAccount` to restrict the trust relationship between cross services and AWS OpsWorks CM to servers in a specific account, which is less restrictive\. The following is an example\. Note that if you use both condition keys, the account IDs must be the same\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "opsworks-cm.amazonaws.com"
         },
         "Action": "sts:AssumeRole",
         "Condition": {
           "StringEquals": {
             "aws:SourceAccount": "123456789012"
           },
           "ArnEquals": {
             "aws:SourceArn": "arn:aws:opsworks-cm:us-east-2:123456789012:server/my-opsworks-server/EXAMPLEabcd-1234-efghEXAMPLE-ID"
           }
         }
       }
     ]
   }
   ```

1. When you are finished adding condition keys, choose **Update trust policy**\.

The following are additional examples of roles that limit access to AWS OpsWorks CM servers by using `aws:SourceArn` and `aws:SourceAccount`\.

**Topics**
+ [Example: Accessing AWS OpsWorks CM servers in a specific region](#confused-deputy-opsworks-cm-example-1)
+ [Example: Adding more than one server ARN to `aws:SourceArn`](#confused-deputy-opsworks-cm-example-2)

### Example: Accessing AWS OpsWorks CM servers in a specific region<a name="confused-deputy-opsworks-cm-example-1"></a>

The following role trust relationship statement accesses any AWS OpsWorks CM servers in the US East \(Ohio\) Region \(`us-east-2`\)\. Note that the region is specified in the ARN value of `aws:SourceArn`, but the server ID value is a wildcard \(\*\)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "opsworks-cm.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "123456789012"
        },
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:opsworks-cm:us-east-2:123456789012:server/*"
        }
      }
    }
  ]
}
```

### Example: Adding more than one server ARN to `aws:SourceArn`<a name="confused-deputy-opsworks-cm-example-2"></a>

The following example limits access to an array of two AWS OpsWorks CM servers in account ID 123456789012\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "opsworks-cm.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "123456789012"
        },
        "ArnEquals": {
          "aws:SourceArn": [
             "arn:aws:opsworks-cm:us-east-2:123456789012:server/my-chef-server/unique_ID",
             "arn:aws:opsworks-cm:us-east-2:123456789012:server/my-puppet-server/unique_ID"
           ]
       }
      }
    }
  ]
}
```