# Instance Registration Policies<a name="registered-instances-register-registering-template"></a>

The `AWSOpsWorksRegisterCLI_EC2` and `AWSOpsWorksRegisterCLI_OnPremises` policies provide the correct permissions for registering EC2 and on\-premises instances, respectively\. You add `AWSOpsWorksRegisterCLI_EC2` to your IAM user to register EC2 instances, but add `AWSOpsWorksRegisterCLI_OnPremises` to your IAM user to register on\-premises instances\. To use these policies, you must be running at least version 1\.16\.180 of the AWS CLI or newer\.

## The `AWSOpsWorksRegisterCLI_EC2` Policy<a name="instance-profile-policy"></a>

Add `AWSOpsWorksRegisterCLI_EC2` to your IAM user to register EC2 instances\. You should use this profile if you plan to register only EC2 instances\. When you use this policy, permissions are provided by the EC2 instance's instance profile\.

```
{
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
            "opsworks:AssignInstance",
            "opsworks:CreateLayer",
            "opsworks:DeregisterInstance",
            "opsworks:DescribeInstances",
            "opsworks:DescribeStackProvisioningParameters",
            "opsworks:DescribeStacks",
            "opsworks:UnassignInstance"
          ],
          "Resource": [
            "*"
          ]
        },
        {
          "Effect": "Allow",
          "Action": [
            "ec2:DescribeInstances"
          ],
          "Resource": [
            "*"
          ]
        }
      ]
    }
```

## The `AWSOpsWorksRegisterCLI_OnPremises` Policy<a name="register-onprem-policy"></a>

Add `AWSOpsWorksRegisterCLI_OnPremises` to your IAM user to register on\-premises instances\. This policy includes IAM permissions, such as `AttachUserPolicy`, but the resources on which those permissions work are restricted\.

```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
            "opsworks:AssignInstance",
            "opsworks:CreateLayer",
            "opsworks:DeregisterInstance",
            "opsworks:DescribeInstances",
            "opsworks:DescribeStackProvisioningParameters",
            "opsworks:DescribeStacks",
            "opsworks:UnassignInstance"
          ],
          "Resource": [
            "*"
          ]
        },
        {
          "Effect": "Allow",
          "Action": [
            "ec2:DescribeInstances"
          ],
          "Resource": [
            "*"
          ]
        },
        {
          "Effect": "Allow",
          "Action": [
            "iam:CreateGroup",
            "iam:AddUserToGroup"
          ],
          "Resource": [
            "arn:aws:iam::*:group/AWS/OpsWorks/OpsWorks-*"
          ]
        },
        {
          "Effect": "Allow",
          "Action": [
            "iam:CreateUser",
            "iam:CreateAccessKey"
          ],
          "Resource": [
            "arn:aws:iam::*:user/AWS/OpsWorks/OpsWorks-*"
          ]
        },
        {
          "Effect": "Allow",
          "Action": [
            "iam:AttachUserPolicy"
          ],
          "Resource": [
            "arn:aws:iam::*:user/AWS/OpsWorks/OpsWorks-*"
          ],
          "Condition": {
            "ArnEquals": 
              {
                "iam:PolicyARN": "arn:aws:iam::aws:policy/AWSOpsWorksInstanceRegistration"
              }
            }
        }
      ]
    }
```

## \(Deprecated\) The `AWSOpsWorksRegisterCLI` Policy<a name="registercli-policy"></a>

**Important**  
The `AWSOpsWorksRegisterCLI` policy has been deprecated, and cannot be used to register new instances\. It is available only for backward compatibility on instances that have already been registered\. The `AWSOpsWorksRegisterCLI` policy includes many IAM permissions including `CreateUser`, `PutUserPolicy`, and `AddUserToGroup`\. Because these are administrator\-level permissions, you should only assign the `AWSOpsWorksRegisterCLI` policy to trusted administrative users\.

The following example shows the `AWSOpsWorksRegisterCLI` policy, which grants permissions to run `register` for on\-premises instances\. To register Amazon EC2 instances, we recommend that you use [`AWSOpsWorksRegisterCLI_EC2`](#instance-profile-policy) instead\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "opsworks:AssignInstance",
        "opsworks:CreateStack",
        "opsworks:CreateLayer",
        "opsworks:DeregisterInstance",
        "opsworks:DescribeInstances",
        "opsworks:DescribeStackProvisioningParameters",
        "opsworks:DescribeStacks",
        "opsworks:UnassignInstance"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:AddUserToGroup",
        "iam:CreateAccessKey",
        "iam:CreateGroup",
        "iam:CreateUser",
        "iam:ListInstanceProfiles",
        "iam:PassRole",
        "iam:PutUserPolicy"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```