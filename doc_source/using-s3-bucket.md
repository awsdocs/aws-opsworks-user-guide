# Step 1: Create an Amazon S3 Bucket<a name="using-s3-bucket"></a>

You must first create an Amazon S3 bucket\. You can do this directly by using the Amazon S3 console, API, or CLI, but a simpler way to create resources is often to use a AWS CloudFormation template\. The following template creates an Amazon S3 bucket for this example and sets up [instance profile](http://docs.aws.amazon.com/IAM/latest/UserGuide/instance-profiles.html) with an [IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/WorkingWithRoles.html) that grants unrestricted access to the bucket\. You can then use a layer setting to attach the instance profile to the stack's application server instances, which allows the application to access the bucket, as described later\. The usefulness of instance profiles isn't limited to Amazon S3; they are valuable for integrating a variety of AWS services\. 

```
{
   "AWSTemplateFormatVersion" : "2010-09-09",
   "Resources" : {
      "AppServerRootRole": {
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
      "AppServerRolePolicies": {
         "Type": "AWS::IAM::Policy",
         "Properties": {
            "PolicyName": "AppServerS3Perms",
            "PolicyDocument": {
               "Statement": [ {
                  "Effect": "Allow",
                  "Action": "s3:*",
                  "Resource": { "Fn::Join" : ["", [ "arn:aws:s3:::", { "Ref" : "AppBucket" } , "/*" ]
                  ] }
               } ]
            },
            "Roles": [ { "Ref": "AppServerRootRole" } ]
         }
      },
      "AppServerInstanceProfile": {
         "Type": "AWS::IAM::InstanceProfile",
         "Properties": {
            "Path": "/",
            "Roles": [ { "Ref": "AppServerRootRole" } ]
         }
      },
     "AppBucket" : {
      "Type" : "AWS::S3::Bucket"
      }
   },
   "Outputs" : {
       "BucketName" : {
           "Value" : { "Ref" : "AppBucket" }
       },
       "InstanceProfileName" : {
           "Value" : { "Ref" : "AppServerInstanceProfile" }
       }
   }
}
```

Several things happen when you launch the template:
+ The `[AWS::S3::Bucket](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html)` resource creates an Amazon S3 bucket\.
+ The `[AWS::IAM::InstanceProfile](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-instanceprofile.html)` resource creates an instance profile that will be assigned to the application server instances\.
+ The `[AWS::IAM::Role](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html)` resource creates the instance profile's role\.
+ The `[AWS::IAM::Policy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-policy.html)` resource sets the role's permissions to allow unrestricted access to Amazon S3 buckets\.
+ The `Outputs` section displays the bucket and instance profile names in AWS CloudFormation console after you have launched the template\.

  You will need these values to set up your stack and app\.

For more information on how to create AWS CloudFormation templates, see [Learn Template Basics](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/gettingstarted.templatebasics.html)\.

**To create the Amazon S3 bucket**

1. Copy the example template to a text file on your system\.

   This example assumes that the file is named `appserver.template`\.

1. Open the [AWS CloudFormation](https://console.aws.amazon.com/cloudformation/) console and choose **Create Stack**\.

1. In the **Stack Name** box, enter the stack name\.

   This example assumes that the name is **AppServer**\.

1. Choose **Upload template file**, choose **Browse**, select the `appserver.template` file that you created in Step 1, and then choose **Next Step**\.

1. On the **Specify Parameters** page, select **I acknowledge that this template may create IAM resources**, then choose **Next Step** on each page of the wizard until you reach the end\. Choose **Create**\. 

1. After the **AppServer** stack reaches **CREATE\_COMPLETE** status, select it and choose the **Outputs** tab\.

   You might need to refresh a few times to update the status\.

1. On the **Outputs** tab, record the **BucketName** and **InstanceProfileName** values for later use\.

**Note**  
AWS CloudFormation uses the term *stack* to refer to the collection of resources that are created from a template; it is not the same as an AWS OpsWorks Stacks stack\.