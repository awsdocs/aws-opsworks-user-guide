# Create an AWS OpsWorks for Puppet Enterprise Master by using AWS CloudFormation<a name="opspup-create-server-cfn"></a>

AWS OpsWorks for Puppet Enterprise lets you run a [Puppet Enterprise](https://puppet.com/products/puppet-enterprise) server in AWS\. You can provision a Puppet Enterprise master server in about 15 minutes\.

The following walkthrough helps you create a Puppet master in OpsWorks for Puppet Enterprise by creating a stack in AWS CloudFormation\.

**Topics**
+ [Prerequisites](#opspup-create-server-cfn-prereqs)
+ [Create a Puppet Enterprise Master in AWS CloudFormation](#opspup-create-server-cfn-main)

## Prerequisites<a name="opspup-create-server-cfn-prereqs"></a>

Before you create a new Puppet master, create the resources outside of OpsWorks for Puppet Enterprise that you'll need to access and manage your Puppet master\. For more information, see [Prerequisites](gettingstarted-opspup.md#gettingstarted-opspup-prereqs) in the Getting Started section of this guide\.

Review the [OpsWorks\-CM section](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-opsworkscm-server.html) of the *AWS CloudFormation User Guide* Template Reference to learn about the supported and required values in the AWS CloudFormation template that you use to create your server\.

## Create a Puppet Enterprise Master in AWS CloudFormation<a name="opspup-create-server-cfn-main"></a>

This section describes how to use an AWS CloudFormation template to build a stack that creates an OpsWorks for Puppet Enterprise master server\. You can do this by using the AWS CloudFormation console or the AWS CLI\. An [example AWS CloudFormation template](samples/opsworkscm-puppet-server.zip) is available for you to use to build an OpsWorks for Puppet Enterprise server stack\. Be sure to update the example template with your own server name, IAM roles, instance profile, server description, backup retention count, and maintenance options\. For more information about these options, see [Create a Puppet Enterprise Master by using the AWS Management Console](gettingstarted-opspup-create.md#gettingstarted-opspup-create-console) in the Getting Started section of this guide\.

**Topics**
+ [Create a Puppet Enterprise Master by using AWS CloudFormation \(Console\)](#opspup-create-server-cfn-console)
+ [Create a Puppet Enterprise Master by using AWS CloudFormation \(CLI\)](#opspup-create-server-cfn-cli)

### Create a Puppet Enterprise Master by using AWS CloudFormation \(Console\)<a name="opspup-create-server-cfn-console"></a>

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. On the AWS CloudFormation home page, choose **Create stack**\.

1. In **Prerequisite \- Prepare template**, if you are using the [example AWS CloudFormation template](samples/opsworkscm-puppet-server.zip), choose **Template is ready**\.

1. In **Specify template**, choose the source of your template\. For this walkthrough, choose **Upload a template file**, and upload an AWS CloudFormation template that creates a Puppet Enterprise server\. Browse for your template file, and then choose **Next**\.

   An AWS CloudFormation template can be in either YAML or JSON format\. An [example AWS CloudFormation template](samples/opsworkscm-puppet-server.zip) is available for you to use; be sure to replace example values with your own\. You can use the AWS CloudFormation template designer to build a new template or validate an existing one\. For more information about how to do this, see [AWS CloudFormation Designer Interface Overview](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer-overview.html) in the *AWS CloudFormation User Guide*\.  
![\[CloudFormation Create stack page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cfn_select_template.png)

1. On the **Specify stack details** page, enter a name for your stack\. This won't be the same as the name of your server, it is only a stack name\. In the **Parameters** area, enter an administrator password for signing in to the Puppet Enterprise console webpage\. The password must use between 8 and 32 ASCII characters\. Choose **Next**\.  
![\[Specify Details page in CloudFormation\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cfn_template_params.png)

1. On the **Options** page, you can add tags to the server you're creating with the stack, and choose an IAM role for creating resources if you have not already specified an IAM role to use in your template\. When you're finished specifying options, choose **Next**\. For more information about advanced options such as rollback triggers, see [Setting AWS CloudFormation Stack Options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html) in the *AWS CloudFormation User Guide*\.

1. On the **Review** page, review your choices\. When you are ready to create the server stack, choose **Create**\.

   While you are waiting for AWS CloudFormation to create the stack, view the stack creation status\. If stack creation fails, review the error messages shown in the console to help you resolve the issues\. For more information about troubleshooting errors in AWS CloudFormation stacks, see [Troubleshooting Errors](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html#troubleshooting-errors) in the *AWS CloudFormation User Guide*\.

   When server creation is finished, your Puppet master is available on the OpsWorks for Puppet Enterprise home page, with a status of **online**\. After the server is online, the Puppet Enterprise console is available on the server's domain, at a URL in the following format: `https://your_server_name-randomID.region.opsworks-cm.io`\.

### Create a Puppet Enterprise Master by using AWS CloudFormation \(CLI\)<a name="opspup-create-server-cfn-cli"></a>

If your local computer is not already running the AWS CLI, download and install the AWS CLI by following [installation instructions](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\. This section does not describe all parameters that you can use with the create\-stack command\. For more information about create\-stack parameters, see [https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html) in the *AWS CLI Reference*\.

1. Be sure to complete the [Prerequisites](gettingstarted-opspup.md#gettingstarted-opspup-prereqs) for creating an OpsWorks for Puppet Enterprise master\.

1. Create a service role and an instance profile\. AWS OpsWorks provides an AWS CloudFormation template that you can use to create both\. Run the following AWS CLI command to create an AWS CloudFormation stack that creates the service role and instance profile for you\.

   ```
   aws cloudformation create-stack --stack-name OpsWorksCMRoles --template-url https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-roles.yaml --capabilities CAPABILITY_NAMED_IAM
   ```

   After AWS CloudFormation finishes creating the stack, find and copy the ARNs of service roles in your account\.

   ```
   aws iam list-roles --path-prefix "/service-role/" --no-paginate
   ```

   In the results of the `list-roles` command, look for service role and instance profile entries that resemble the following\. Make a note of the ARNs of the service role and instance profile, and add them to the AWS CloudFormation template that you are using to create your Puppet master server stack\.

   ```
   {
       "AssumeRolePolicyDocument": {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Action": "sts:AssumeRole",
                   "Effect": "Allow",
                   "Principal": {
                       "Service": "ec2.amazonaws.com"
                   }
               }
           ]
       },
       "RoleId": "AROZZZZZZZZZZQG6R22HC",
       "CreateDate": "2018-01-05T20:42:20Z",
       "RoleName": "aws-opsworks-cm-ec2-role",
       "Path": "/service-role/",
       "Arn": "arn:aws:iam::000000000000:role/service-role/aws-opsworks-cm-ec2-role"
   },
   {
       "AssumeRolePolicyDocument": {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Action": "sts:AssumeRole",
                   "Effect": "Allow",
                   "Principal": {
                       "Service": "opsworks-cm.amazonaws.com"
                   }
               }
           ]
       },
       "RoleId": "AROZZZZZZZZZZZZZZZ6QE",
       "CreateDate": "2018-01-05T20:42:20Z",
       "RoleName": "aws-opsworks-cm-service-role",
       "Path": "/service-role/",
       "Arn": "arn:aws:iam::000000000000:role/service-role/aws-opsworks-cm-service-role"
   }
   ```

1. Create the OpsWorks for Puppet Enterprise master by running the create\-stack command again\.
   + Replace *stack\_name* with the name of your stack\. This is the name of the AWS CloudFormation stack, not your Puppet master\. The Puppet master name is the value of `ServerName` in your AWS CloudFormation template\.
   + Replace *template* with the path to your template file, and the extension *yaml or json* with `.yaml` or `.json` as appropriate\.
   + The values for `--parameters` correspond to [EngineAttributes](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-opsworkscm-server.html#cfn-opsworkscm-server-engineattributes) from the [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) API\. For Puppet, the following are user\-provided engine attributes to create a server\. r10k engine attributes connect your Puppet master to a code repository to manage the server’s environment configuration\. For more information about r10k engine attributes, see [Managing code with r10k](https://puppet.com/docs/pe/2017.3/r10k.html) in the Puppet Enterprise documentation\.
     + `PUPPET_ADMIN_PASSWORD`, an administrator password for signing in to the Puppet Enterprise console webpage\. The password must use between 8 and 32 ASCII characters, and requires at least one upper case letter, one lower case letter, one number, and one special character\.
     + `PUPPET_R10K_REMOTE`, the URL of your control repository \(for example, ssh://git@your\.git\-repo\.com:user/control\-repo\.git\)\. Specifying an r10k remote opens TCP port 8170\.
     + `PUPPET_R10K_PRIVATE_KEY`\. If you are using a private Git repository, add PUPPET\_R10K\_PRIVATE\_KEY to specify an SSH URL and a PEM\-encoded private SSH key\.

   ```
   aws cloudformation create-stack --stack-name stack_name --template-body file://template.yaml or json --parameters ParameterKey=AdminPassword,ParameterValue="password"
   ```

   The following is an example\.

   ```
   aws cloudformation create-stack --stack-name "OpsWorksCMPuppetServerStack" --template-body file://opsworkscm-puppet-server.json --parameters ParameterKey=AdminPassword,ParameterValue="09876543210Ab#"
   ```

   The following example specifies r10k engine attributes as parameters, when they are not provided in the AWS CloudFormation template\. An example template that includes the r10k engine attributes, `puppet-server-param-attributes.yaml`, is included in the [example AWS CloudFormation templates](samples/opsworkscm-puppet-server.zip)\.

   ```
   aws cloudformation create-stack --stack-name MyPuppetStack --template-body file://puppet-server-param-attributes.yaml --parameters ParameterKey=AdminPassword,ParameterValue="superSecret1%3" ParameterKey=R10KRemote,ParameterValue="https://www.yourRemote.com" ParameterKey=R10KKey,ParameterValue="$(cat puppet-r10k.pem)"
   ```

   The following example specifies r10k engine attributes and their values in the AWS CloudFormation template; the command only needs to point to the template file\. The template specified as the value of `--template-body`, `puppet-server-in-file-attributes.yaml`, is included in the [example AWS CloudFormation templates](samples/opsworkscm-puppet-server.zip)\.

   ```
   aws cloudformation create-stack --stack-name MyPuppetStack --template-body file://puppet-server-in-file-attributes.yaml
   ```

1. \(Optional\) To get stack creation status, run the following command\.

   ```
   aws cloudformation describe-stacks --stack-name stack_name 
   ```

1. When stack creation has finished, go on to the next section, [Configure the Puppet Master Using the Starter Kit](opspup-starterkit.md)\. If stack creation fails, review the error messages shown in the console to help you resolve the issues\. For more information about troubleshooting errors in AWS CloudFormation stacks, see [Troubleshooting Errors](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html#troubleshooting-errors) in the *AWS CloudFormation User Guide*\.