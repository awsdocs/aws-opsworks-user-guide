# Specifying Permissions for Apps Running on EC2 instances<a name="opsworks-security-appsrole"></a>

If the applications running on your stack's Amazon EC2 instances need to access other AWS resources, such as Amazon S3 buckets, they must have appropriate permissions\. To confer those permissions, you use an instance profile\. You can specify an instance profile for each instance when you [create an AWS OpsWorks Stacks stack](workingstacks-creating.md)\. 

![\[Advanced option in Add Stack page.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add-stack-defaultiaminstanceproflie.png)

You can also specify a profile for a layer's instances by [editing the layer configuration](workinglayers-basics-edit.md)\.

The instance profile specifies an IAM role\. Applications running on the instance can assume that role to access AWS resources, subject to the permissions that are granted by the role's policy\. For more information about how an application assumes a role, see [Assuming the Role Using an API Call](http://docs.aws.amazon.com/IAM/latest/UserGuide/roles-assume-role.html)\.

You can create an instance profile in any of the following ways:
+ Have AWS OpsWorks Stacks create a new profile when you create a stack\.

  The profile will be named something like `aws-opsworks-ec2-role` and will have a trust relationship but no policy\. 
+ Use the IAM console or API to create a profile\.

  For more information, see [Roles \(Delegation and Federation\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/WorkingWithRoles.html)\.
+ Use an AWS CloudFormation template to create a profile\.

  For some examples of how to include IAM resources in a template, see [Identity and Access Management \(IAM\) Template Snippets](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-iam.html)\.

An instance profile must have a trust relationship and an attached policy that grants permissions to access AWS resources\. Instance profiles created by AWS OpsWorks Stacks have the following trust relationship\.

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

The instance profile must have this trust relationship for AWS OpsWorks Stacks to act on your behalf\. If you use the default service role, do not modify the trust relationship\. If you are creating a custom service role, specify the trust relation ship as follows: 
+ If you are using the **Create Role** wizard in the [IAM console](https://console.aws.amazon.com/iam/home#roles), specify the **Amazon EC2** role type under **AWS Service Roles** on the wizard's second page\. 
+ If you are using a AWS CloudFormation template, you can add something like the following to your template's **Resources** section\.

  ```
  "Resources": {
        "OpsWorksEC2Role": {
           "Type": "AWS::IAM::Role",
           "Properties": {
              "AssumeRolePolicyDocument": {
                 "Statement": [ {
                    "Effect": "Allow",
                    "Principal": {
                       "Service": [ "ec2.amazonaws.com" ]
                    },
                    "Action": [ "sts:AssumeRole" ]
                 } ]
              },
              "Path": "/"
           }
        },
        "RootInstanceProfile": {
           "Type": "AWS::IAM::InstanceProfile",
           "Properties": {
              "Path": "/",
              "Roles": [ {
                 "Ref": "OpsWorksEC2Role"
              } ]
           }
        }
     }
  }
  ```

If you create your own instance profile, you can attach an appropriate policy to the profile's role at that time\. If you have AWS OpsWorks Stacks create an instance profile for you when you create the stack, it does not have an attached policy and grants no permissions\. After you have created the stack, you must use the [IAM console](https://console.aws.amazon.com/iam/) or API to attach an appropriate policy to the profile's role\. For example, the following policy grants full access to any Amazon S3 bucket\.

```
{
  "Version": "2012-10-17",
  "Statement": [ {
    "Effect": "Allow",
    "Action": "s3:*",
    "Resource": "*"
  }
  ] 
}
```

For an example of how to create and use an instance profile, see [Using an Amazon S3 Bucket](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted.walkthrough.photoapp.html)\.

If your application uses an instance profile to call the AWS OpsWorks Stacks API from an EC2 instance, the policy must allow the `iam:PassRole` action in addition to the appropriate actions for AWS OpsWorks Stacks and other AWS services\. The `iam:PassRole` permission allows AWS OpsWorks Stacks to assume the service role on your behalf\. For more information about the AWS OpsWorks Stacks API, see [AWS OpsWorks API Reference](http://docs.aws.amazon.com/opsworks/latest/APIReference/Welcome.html)\.

The following is an example of an IAM policy that allows you to call any AWS OpsWorks Stacks action from an EC2 instance, as well as any Amazon EC2 or Amazon S3 action\.

```
{
  "Version": "2012-10-17",
  "Statement": [ {
    "Effect": "Allow",
    "Action": [ "ec2:*", "s3:*", "opsworks:*", "iam:PassRole"],
    "Resource": "*"
  }
  ] 
}
```

**Note**  
If you do not allow `iam:PassRole`, any attempt to call an AWS OpsWorks Stacks action fails with an error like the following:  

```
User: arn:aws:sts::123456789012:federated-user/Bob is not authorized
to perform: iam:PassRole on resource:
arn:aws:sts::123456789012:role/OpsWorksStackIamRole
```

For more information about using roles on an EC2 instance for permissions, see [Granting Applications that Run on Amazon EC2 Instances Access to AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/role-usecase-ec2app.html) in the *AWS Identity and Access Management User Guide*\.