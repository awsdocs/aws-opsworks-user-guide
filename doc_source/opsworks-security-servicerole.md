# Allowing AWS OpsWorks Stacks to Act on Your Behalf<a name="opsworks-security-servicerole"></a>

AWS OpsWorks Stacks needs to interact with a variety of AWS services on your behalf\. For example, AWS OpsWorks Stacks interacts with Amazon EC2 to create instances and with Amazon CloudWatch to get monitoring statistics\. When you create a stack, you specify an IAM role, usually called a service role, that grants AWS OpsWorks Stacks the appropriate permissions\.

![\[IAM role list in Add stack page.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add-stack-iamrole.png)

When you specify a new stack's service role, you can do one of the following:
+ Have AWS OpsWorks Stacks create a new service role with a standard set of permissions\.

  The role is named `aws-opsworks-service-role`, or similar\.
+ Specify a standard service role that you created earlier\.

  You can usually create a standard service role when you create your first stack, and then use that role for all subsequent stacks\.
+ Specify a custom service role that you created by using the IAM console or API\.

  This approach is useful if you want to grant AWS OpsWorks Stacks more limited permissions than the standard service role\.

**Note**  
To create your first stack, you must have the permissions defined in the IAM **AdministratorAccess** policy template\. These permissions allow AWS OpsWorks Stacks to create a new IAM service role and allow you to import users, [as described earlier](opsworks-security-users-manage-import.md)\. For all subsequent stacks, users can select the service role created for the first stack; they don't require full administrative permissions to create a stack\.

The standard service role grants the following permissions:
+ Perform all Amazon EC2 actions \(`ec2:*`\)\.
+ Get CloudWatch statistics \(`cloudwatch:GetMetricStatistics`\)\. 
+ Use Elastic Load Balancing to distribute traffic to servers \(`elasticloadbalancing:*`\)\.
+ Use an Amazon RDS instance as a database server \(`rds:*`\)\.
+ Use IAM roles \(`iam:PassRole`\) to provide secure communication between AWS OpsWorks Stacks and your Amazon EC2 instances\.

If you create a custom service role, you must ensure that it grants all the permissions that AWS OpsWorks Stacks needs to manage your stack\. The following JSON sample is the policy statement for the standard service role; a custom service role should include at least the following permissions in its policy statement\.

```
{
    "Statement": [
        {
            "Action": [
                "ec2:*",
                "iam:PassRole",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:DescribeAlarms",
                "ecs:*",
                "elasticloadbalancing:*",
                "rds:*"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        }
    ]
}
```

A service role also has a trust relationship\. Service roles created by AWS OpsWorks Stacks have the following trust relationship\.

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "opsworks.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

The service role must have this trust relationship for AWS OpsWorks Stacks to act on your behalf\. If you use the default service role, do not modify the trust relationship\. If you are creating a custom service role, specify the trust relationship as follows: 
+ If you are using the **Create role** wizard in the [IAM console](https://console.aws.amazon.com/iam/home#roles), specify the **AWS Opsworks** role type under **AWS Service Roles** on the wizard's second page\. Note that the **AWSOpsWorksRole** policy that you are prompted to add by default in the wizard has the appropriate trust relationship, and allows AWS OpsWorks to perform some actions on your behalf, but does not allow you to create new resources such as instances, Elastic Load Balancing load balancers, or Amazon ECS clusters\. To grant AWS OpsWorks permissions to create those resources on your behalf, attach a custom policy to your role that adds the following allowed actions to what is available in the **AWSOpsWorksRole** policy\.

  ```
  "ec2:*",
  "ecs:*",
  "elasticloadbalancing:*",
  "rds:*"
  ```
+ If you are using a AWS CloudFormation template, you can add something like the following to your template's **Resources** section\.

  ```
  "Resources": {
    "OpsWorksServiceRole": {
        "Type": "AWS::IAM::Role",
        "Properties": {
          "AssumeRolePolicyDocument": {
              "Statement": [ {
                "Effect": "Allow",
                "Principal": {
                    "Service": [ "opsworks.amazonaws.com" ]
                },
                "Action": [ "sts:AssumeRole" ]
              } ]
          },
          "Path": "/",
          "Policies": [ {
              "PolicyName": "opsworks-service",
              "PolicyDocument": {
                ...
              } ]
          }
        },
     }
  }
  ```