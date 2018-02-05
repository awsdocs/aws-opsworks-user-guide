# The AWSOpsWorksRegisterCLI Policy<a name="registered-instances-register-registering-template"></a>

The following example shows the AWSOpsWorksRegisterCLI policy, which grants permissions to use `register` to for on\-premises and Amazon EC2 instances\. Note that the Amazon EC2 permissions are required only for registering Amazon EC2 instances\.

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