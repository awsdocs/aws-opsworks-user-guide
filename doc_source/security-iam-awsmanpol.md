# AWS managed policies for AWS OpsWorks Configuration Management<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

## AWS managed policy: `AWSOpsWorksCMServiceRole`<a name="security-iam-awsmanpol-AWSOpsWorksCMServiceRole"></a>

You can attach `AWSOpsWorksCMServiceRole` to your IAM entities\. OpsWorks CM also attaches this policy to a service role that allows OpsWorks CM to perform actions on your behalf\.

This policy grants *administrative* permissions that allow OpsWorks CM administrators to create, manage, and delete OpsWorks CM servers and backups\.

**Permissions details**

This policy includes the following permissions\.
+ `opsworks-cm` – Allows principals to delete existing servers, and start maintenance runs\.
+ `acm` – Allows principals to delete or import certificates from AWS Certificate Manager that let users connect to an OpsWorks CM server\.
+ `cloudformation` – Allows OpsWorks CM to create and manage AWS CloudFormation stacks when principals create, update, or delete OpsWorks CM servers\.
+ `ec2` – Allows OpsWorks CM to launch, provision, update, and terminate Amazon Elastic Compute Cloud instances when principals create, update, or delete OpsWorks CM servers\.
+ `iam` – Allows OpsWorks CM to create service roles that are required for creating and managing OpsWorks CM servers\.
+ `tag` – Allows principals to apply and remove tags from OpsWorks CM resources, including servers and backups\.
+ `s3` – Allows OpsWorks CM to create Amazon S3 buckets for storing server backups, manage objects in S3 buckets on principal request \(for example, deleting a backup\), and delete buckets\.
+ `secretsmanager` – Allows OpsWorks CM to create and manage Secrets Manager secrets, and apply or remove tags from secrets\.
+ `ssm` – Allows OpsWorks CM to use Systems Manager Run Command on the instances that are OpsWorks CM servers\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::aws-opsworks-cm-*"
            ],
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteObject",
                "s3:DeleteBucket",
                "s3:GetObject",
                "s3:ListBucket",
                "s3:PutBucketPolicy",
                "s3:PutObject",
                "s3:GetBucketTagging",
                "s3:PutBucketTagging"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Action": [
                "tag:UntagResources",
                "tag:TagResources"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Action": [
                "ssm:DescribeInstanceInformation",
                "ssm:GetCommandInvocation",
                "ssm:ListCommandInvocations",
                "ssm:ListCommands"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringLike": {
                    "ssm:resourceTag/aws:cloudformation:stack-name": "aws-opsworks-cm-*"
                }
            },
            "Action": [
                "ssm:SendCommand"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:*::document/*",
                "arn:aws:s3:::aws-opsworks-cm-*"
            ],
            "Action": [
                "ssm:SendCommand"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Action": [
                "ec2:AllocateAddress",
                "ec2:AssociateAddress",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:CreateImage",
                "ec2:CreateSecurityGroup",
                "ec2:CreateSnapshot",
                "ec2:CreateTags",
                "ec2:DeleteSecurityGroup",
                "ec2:DeleteSnapshot",
                "ec2:DeregisterImage",
                "ec2:DescribeAccountAttributes",
                "ec2:DescribeAddresses",
                "ec2:DescribeImages",
                "ec2:DescribeInstanceStatus",
                "ec2:DescribeInstances",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSnapshots",
                "ec2:DescribeSubnets",
                "ec2:DisassociateAddress",
                "ec2:ReleaseAddress",
                "ec2:RunInstances",
                "ec2:StopInstances"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringLike": {
                    "ec2:ResourceTag/aws:cloudformation:stack-name": "aws-opsworks-cm-*"
                }
            },
            "Action": [
                "ec2:TerminateInstances",
                "ec2:RebootInstances"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:opsworks-cm:*:*:server/*"
            ],
            "Action": [
                "opsworks-cm:DeleteServer",
                "opsworks-cm:StartMaintenance"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:cloudformation:*:*:stack/aws-opsworks-cm-*"
            ],
            "Action": [
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStackEvents",
                "cloudformation:DescribeStackResources",
                "cloudformation:DescribeStacks",
                "cloudformation:UpdateStack"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:iam::*:role/aws-opsworks-cm-*",
                "arn:aws:iam::*:role/service-role/aws-opsworks-cm-*"
            ],
            "Action": [
                "iam:PassRole"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": "*",
            "Action": [
                "acm:DeleteCertificate",
                "acm:ImportCertificate"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": "arn:aws:secretsmanager:*:*:opsworks-cm!aws-opsworks-cm-secrets-*",
            "Action": [
                "secretsmanager:CreateSecret",
                "secretsmanager:GetSecretValue",
                "secretsmanager:UpdateSecret",
                "secretsmanager:DeleteSecret",
                "secretsmanager:TagResource",
                "secretsmanager:UntagResource"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "ec2:DeleteTags",
            "Resource": [
                "arn:aws:ec2:*:*:instance/*",
                "arn:aws:ec2:*:*:elastic-ip/*",
                "arn:aws:ec2:*:*:security-group/*"
            ]
        }
    ]
}
```

## AWS managed policy: `AWSOpsWorksCMInstanceProfileRole`<a name="security-iam-awsmanpol-AWSOpsWorksCMInstanceProfileRole"></a>

You can attach `AWSOpsWorksCMInstanceProfileRole` to your IAM entities\. OpsWorks CM also attaches this policy to a service role that allows OpsWorks CM to perform actions on your behalf\. 

This policy grants *administrative* permissions that allow the Amazon EC2 instances that are used as OpsWorks CM servers to get information from AWS CloudFormation and AWS Secrets Manager, and store server backups in Amazon S3 buckets\.

**Permissions details**

This policy includes the following permissions\.
+ `acm` – Allows OpsWorks CM server EC2 instances to get certificates from AWS Certificate Manager that let users connect to an OpsWorks CM server\.
+ `cloudformation` – Allows OpsWorks CM server EC2 instances to get information about AWS CloudFormation stacks during the instance creation or update process, and send signals to AWS CloudFormation about its status\.
+ `s3` – Allows OpsWorks CM server EC2 instances to upload and store server backups in S3 buckets, stop or roll back uploads if necessary, and delete backups from S3 buckets\.
+ `secretsmanager` – Allows OpsWorks CM server EC2 instances to get the values of OpsWorks CM related Secrets Manager secrets\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "cloudformation:DescribeStackResource",
                "cloudformation:SignalResource"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        },
        {
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:DeleteObject",
                "s3:GetObject",
                "s3:ListAllMyBuckets",
                "s3:ListBucket",
                "s3:ListMultipartUploadParts",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::aws-opsworks-cm-*",
            "Effect": "Allow"
        },
        {
            "Action": "acm:GetCertificate",
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "arn:aws:secretsmanager:*:*:opsworks-cm!aws-opsworks-cm-secrets-*",
            "Effect": "Allow"
        }
    ]
}
```

## OpsWorks CM updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for OpsWorks CM since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the [OpsWorks CM Document history](history.md) page\.


| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSOpsWorksCMInstanceProfileRole](#security-iam-awsmanpol-AWSOpsWorksCMInstanceProfileRole) \- Updated managed policy  |  OpsWorks CM updated the managed policy that allows the EC2 instances used as OpsWorks CM servers to share information with CloudFormation and Secrets Manager, and manage backups\. The change adds `opsworks-cm!` to the resource name for Secrets Manager secrets, so that OpsWorks CM is allowed to own the secrets\.   | April 23, 2021 | 
|  [AWSOpsWorksCMServiceRole](#security-iam-awsmanpol-AWSOpsWorksCMServiceRole) \- Updated managed policy   |  OpsWorks CM updated the managed policy that allows OpsWorks CM administrators to create, manage, and delete OpsWorks CM servers and backups\. The change adds `opsworks-cm!` to the resource name for Secrets Manager secrets, so that OpsWorks CM is allowed to own the secrets\.   | April 23, 2021 | 
|  OpsWorks CM started tracking changes  |  OpsWorks CM started tracking changes for its AWS managed policies\.  | April 23, 2021 | 