# Step 1: Create a stack, layer, and an instance in AWS OpsWorks Stacks<a name="other-services-cp-chef11-stack"></a>

To use AWS OpsWorks Stacks as a deployment provider for a pipeline, you must first have a stack, a layer, and at least one instance in the layer\. Although you can create a stack in AWS OpsWorks Stacks by following instructions in [Getting Started with Linux Stacks](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-linux.html) or [Getting Started with Windows Stacks](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-windows.html), to save you time, this example uses an AWS CloudFormation template to create a Linux\-based Chef 11\.10 stack, layer, and instance\. The instance created by this template runs Amazon Linux 2016\.03, and has an instance type of `c3.large`\.

**Important**  
The AWS CloudFormation template must be stored and run in the same region as the Amazon S3 bucket to which you later upload your app and the same region in which you later create your pipeline in CodePipeline\. At this time, CodePipeline supports the AWS OpsWorks Stacks provider in the US East \(N\. Virginia\) Region \(us\-east\-1\) only\. All resources in this walkthrough should be created in the US East \(N\. Virginia\) Region\.  
If stack creation fails, you might be approaching the maximum allowed number of IAM roles for your account\. The stack creation can also fail if your account cannot launch instances with a `c3.large` instance type\. For example, if you are using the AWS Free Tier, you might receive an error such as `Root device type: must be included in EBS`\. If your account has limitations on the instance types that you are allowed to create, such as limitations imposed by the AWS Free Tier, try changing the value of the `InstanceType` parameter in the template's instance block to an instance type that your account can use\.

**To create a stack, layer, and instance using AWS CloudFormation**

1. Copy the following AWS CloudFormation template into a new plain\-text document\. Save the file to a convenient location on your local computer, and name it **NewOpsWorksStack\.template**, or another name that is convenient for you\.

   ```
   {
     "AWSTemplateFormatVersion": "2010-09-09",
     "Mappings": {
       "Region2Principal": {
         "us-east-1": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "us-west-2": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "us-west-1": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "eu-west-1": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "ap-southeast-1": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "ap-northeast-1": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "ap-northeast-2": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "ap-southeast-2": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "sa-east-1": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         },
         "cn-north-1": {
           "EC2Principal": "ec2.amazonaws.com.cn",
           "OpsWorksPrincipal": "opsworks.amazonaws.com.cn"
         },
         "eu-central-1": {
           "EC2Principal": "ec2.amazonaws.com",
           "OpsWorksPrincipal": "opsworks.amazonaws.com"
         }
       }
     },
     "Parameters": {
       "EC2KeyPairName": {
   	  "Type": "String",
   	  "Description": "The name of an existing EC2 key pair that allows you to use SSH to connect to the OpsWorks instance."
   	 }
     },
     "Resources": {
   	"CPOpsDeploySecGroup": {
   	  "Type": "AWS::EC2::SecurityGroup",
   	  "Properties": {
   	    "GroupDescription" : "Lets you manage OpsWorks instances deployed to by CodePipeline"
   	  }
   	},
   	"CPOpsDeploySecGroupIngressHTTP": {
   	  "Type": "AWS::EC2::SecurityGroupIngress",
   	  "Properties" : {
   	    "IpProtocol" : "tcp",
           "FromPort" : "80",
           "ToPort" : "80",
           "CidrIp" : "0.0.0.0/0",
   		"GroupId": {
   		  "Fn::GetAtt": [
   		    "CPOpsDeploySecGroup", "GroupId"
   		  ]
   		}
         }
   	},
   	"CPOpsDeploySecGroupIngressSSH": {
   	  "Type": "AWS::EC2::SecurityGroupIngress",
   	  "Properties" : {
   	    "IpProtocol" : "tcp",
           "FromPort" : "22",
           "ToPort" : "22",
           "CidrIp" : "0.0.0.0/0",
   		"GroupId": {
   		  "Fn::GetAtt": [
   		    "CPOpsDeploySecGroup", "GroupId"
   		  ]
   		}		  
   	  }
   	},
   	"MyStack": {
         "Type": "AWS::OpsWorks::Stack",
         "Properties": {
           "Name": {
             "Ref": "AWS::StackName"
           },
           "ServiceRoleArn": {
             "Fn::GetAtt": [
               "OpsWorksServiceRole",
               "Arn"
             ]
           },
   		"ConfigurationManager" : { "Name": "Chef","Version": "11.10" },
   		"DefaultOs": "Amazon Linux 2016.03",
           "DefaultInstanceProfileArn": {
             "Fn::GetAtt": [
               "OpsWorksInstanceProfile",
               "Arn"
             ]
           }
         }
       },
       "MyLayer": {
         "Type": "AWS::OpsWorks::Layer",
         "Properties": {
           "StackId": {
             "Ref": "MyStack"
           },
           "Name": "MyLayer",
           "Type": "php-app",
   		"Shortname": "mylayer",
           "EnableAutoHealing": "true",
           "AutoAssignElasticIps": "false",
           "AutoAssignPublicIps": "true",
   		"CustomSecurityGroupIds": [
   		  {
   		    "Fn::GetAtt": [
                 "CPOpsDeploySecGroup", "GroupId"
   		    ]
             }			
           ]
         },
         "DependsOn": [
           "MyStack",
           "CPOpsDeploySecGroup"
         ]
       },
       "OpsWorksServiceRole": {
         "Type": "AWS::IAM::Role",
         "Properties": {
           "AssumeRolePolicyDocument": {
             "Statement": [
               {
                 "Effect": "Allow",
                 "Principal": {
                   "Service": [
                     {
                       "Fn::FindInMap": [
                         "Region2Principal",
                         {
                           "Ref": "AWS::Region"
                         },
                         "OpsWorksPrincipal"
                       ]
                     }
                   ]
                 },
                 "Action": [
                   "sts:AssumeRole"
                 ]
               }
             ]
           },
           "Path": "/",
           "Policies": [
             {
               "PolicyName": "opsworks-service",
               "PolicyDocument": {
                 "Statement": [
                   {
                     "Effect": "Allow",
                     "Action": [
                       "ec2:*",
                       "iam:PassRole",
                       "cloudwatch:GetMetricStatistics",
                       "elasticloadbalancing:*"
                     ],
                     "Resource": "*"
                   }
                 ]
               }
             }
           ]
         }
       },
       "OpsWorksInstanceProfile": {
         "Type": "AWS::IAM::InstanceProfile",
         "Properties": {
           "Path": "/",
           "Roles": [
             {
               "Ref": "OpsWorksInstanceRole"
             }
           ]
         }
       },
       "OpsWorksInstanceRole": {
         "Type": "AWS::IAM::Role",
         "Properties": {
           "AssumeRolePolicyDocument": {
             "Statement": [
               {
                 "Effect": "Allow",
                 "Principal": {
                   "Service": [
                     {
                       "Fn::FindInMap": [
                         "Region2Principal",
                         {
                           "Ref": "AWS::Region"
                         },
                         "EC2Principal"
                       ]
                     }
                   ]
                 },
                 "Action": [
                   "sts:AssumeRole"
                 ]
               }
             ]
           },
           "Path": "/",
   		"Policies": [
             {
               "PolicyName": "s3-get",
               "PolicyDocument": {
                 "Version": "2012-10-17",
                 "Statement": [
                   {
                     "Effect": "Allow",
                     "Action": [
                       "s3:GetObject"
                     ],
                     "Resource": "*"
                   }
                 ]
               }
             }
           ]
         }
       },
       "myinstance": {
         "Type": "AWS::OpsWorks::Instance",
         "Properties": {
           "LayerIds": [
             {
               "Ref": "MyLayer"
             }
           ],
           "StackId": {
             "Ref": "MyStack"
           },
           "InstanceType": "c3.large",
           "SshKeyName": {
   		  "Ref": "EC2KeyPairName"
   		}
         }
       }
     },
     "Outputs": {
       "StackId": {
         "Description": "Stack ID for the newly created AWS OpsWorks stack",
         "Value": {
           "Ref": "MyStack"
         }
       }
     }
   }
   ```

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. On the AWS CloudFormation home page, choose **Create stack**\.

1. On the **Select Template** page, in the **Choose a template** area, choose **Upload a template to Amazon S3**, and then choose **Browse**\.

1. Browse to the AWS CloudFormation template that you saved in step 1, and then choose **Open**\. On the **Select Template** page, choose **Next**\.  
![\[Select Template page of AWS CloudFormation Create Stack wizard.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_cfstackcreate.png)

1. On the **Specify Details** page, name the stack **MyStack**, or any stack name that is unique to your account\. If you choose a different name for your stack, change the stack name throughout this walkthrough\.

1. In the **Parameters** area, provide the name of an EC2 key pair that you want to use to access your AWS OpsWorks Stacks instance after it has been created\. Choose **Next**\.

1. On the **Options** page, choose **Next**\. \(Settings on this page are not required for this walkthrough\.\)

1. The AWS CloudFormation template that you use in this walkthrough creates IAM roles, an instance profile, and an instance\.
**Important**  
 Before you choose **Create**, choose **Cost** to estimate charges you might incur from AWS for creating resources with this template\.

   If creating IAM resources is acceptable, select the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources** check box, and then choose **Create**\. If creating IAM resources is not acceptable, you cannot continue with this procedure\.

1. On the AWS CloudFormation dashboard, you can view the progress of the creation of the stack\. Before you continue to the next step, wait until **CREATE\_COMPLETE** is displayed in the **Status** column\.  
![\[AWS CloudFormation dashboard showing stack creation.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_createstack.png)

**To verify stack creation in AWS OpsWorks Stacks**

1. Open the AWS OpsWorks console at [https://console\.aws\.amazon\.com/opsworks/](https://console.aws.amazon.com/opsworks/)\.

1. On the AWS OpsWorks Stacks dashboard, view the stack you created\.  
![\[AWS OpsWorks dashboard showing stack creation.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_verifystack.png)

1. Open the stack, and view the layer and instance\. Observe that the layer and instance were created with the names and other metadata provided in the AWS CloudFormation template\. You are ready to upload your app to an Amazon S3 bucket\.