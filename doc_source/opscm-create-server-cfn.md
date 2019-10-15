# Create an AWS OpsWorks for Chef Automate Server by using AWS CloudFormation<a name="opscm-create-server-cfn"></a>

AWS OpsWorks for Chef Automate lets you run a [Chef Automate](https://www.chef.io/automate/) server in AWS\. You can provision a Chef Automate server in about 15 minutes\.

The following walkthrough helps you create a server in AWS OpsWorks for Chef Automate by creating a stack in AWS CloudFormation\.

**Topics**
+ [Prerequisites](#opscm-create-server-cfn-prereqs)
+ [Create a Chef Automate Server in AWS CloudFormation](#opscm-create-server-cfn-main)

## Prerequisites<a name="opscm-create-server-cfn-prereqs"></a>

Before you create a new Chef Automate server, create the resources outside of AWS OpsWorks for Chef Automate that you'll need to access and manage your Chef server\. For more information, see [Prerequisites](gettingstarted-opscm.md#gettingstarted-opscm-prereq) in the Getting Started section of this guide\.

Review the [OpsWorks\-CM section](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-opsworkscm-server.html) of the *AWS CloudFormation User Guide* Template Reference to learn about the supported and required values in the AWS CloudFormation template that you use to create your server\.

Create a password value for the `CHEF_AUTOMATE_ADMIN_PASSWORD` engine attribute\. The password length is a minimum of eight characters, and a maximum of 32\. The password can contain letters, numbers, and special characters `(!/@#$%^+=_)`\. The password must contain at least one lower case letter, one upper case letter, one number, and one special character\. You specify this password in your AWS CloudFormation template, or as the value of the `CHEF_AUTOMATE_ADMIN_PASSWORD` parameter when you are creating your stack\.

Generate a base64\-encoded RSA key pair before you get started creating a Chef Automate server in AWS CloudFormation\. The pair’s public key is the value of `CHEF_AUTOMATE_PIVOTAL_KEY`, the Chef\- specific [EngineAttributes](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-opsworkscm-server.html#cfn-opsworkscm-server-engineattributes) from the [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) API\. This key is provided as the value of **Parameters** in the AWS CloudFormation console, or in the create\-stack command in the AWS CLI\. To generate this key, we suggest the following methods\.
+ On Linux\-based computers, you can generate this key by running the following [OpenSSL](https://www.openssl.org/) command\.

  ```
  openssl genrsa -out pivotal_key_file_name.pem 2048
  ```

  Then, export the RSA public key portion of the pair to a file\. The public key becomes the value of `CHEF_AUTOMATE_PIVOTAL_KEY`\.

  ```
  openssl rsa -in pivotal_key_file_name.pem -out public.pem -outform PEM
  ```
+ On Windows\-based computers, you can use the PuTTYgen utility to generate a base64\-encoded RSA key pair\. For more information, see [PuTTYgen \- Key Generator for PuTTY on Windows](https://www.ssh.com/ssh/putty/windows/puttygen) on SSH\.com\.

## Create a Chef Automate Server in AWS CloudFormation<a name="opscm-create-server-cfn-main"></a>

This section describes how to use an AWS CloudFormation template to build a stack that creates an AWS OpsWorks for Chef Automate server\. You can do this by using the AWS CloudFormation console or the AWS CLI\. An [example AWS CloudFormation template](samples/opsworkscm-server.zip) is available for you to use to build an AWS OpsWorks for Chef Automate server stack\. Be sure to update the example template with your own server name, IAM roles, instance profile, server description, backup retention count, and maintenance options\. You can specify the `CHEF_AUTOMATE_ADMIN_PASSWORD` and `CHEF_AUTOMATE_PIVOTAL_KEY` engine attributes and their values in the AWS CloudFormation template, or provide just the attributes, and then specify values for the attributes in the AWS CloudFormation **Create Stack** wizard or create\-stack command\. For more information about these attributes, see [Create a Chef Automate server in the AWS Management Console](gettingstarted-opscm-create.md#gettingstarted-opscm-create-console) in the Getting Started section of this guide\.

**Topics**
+ [Create a Chef Automate Server by using AWS CloudFormation \(Console\)](#opscm-create-server-cfn-console)
+ [Create a Chef Automate Server by using AWS CloudFormation \(CLI\)](#opscm-create-server-cfn-cli)

### Create a Chef Automate Server by using AWS CloudFormation \(Console\)<a name="opscm-create-server-cfn-console"></a>

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. On the AWS CloudFormation home page, choose **Create stack**\.

1. In **Prerequisite \- Prepare template**, if you are using the [example AWS CloudFormation template](samples/opsworkscm-server.zip), choose **Template is ready**\.

1. In **Specify template**, choose the source of your template\. For this walkthrough, choose **Upload a template file**, and upload an AWS CloudFormation template that creates a Chef Automate server\. Browse for your template file, and then choose **Next**\.

   An AWS CloudFormation template can be in either YAML or JSON format\. An [example AWS CloudFormation template](samples/opsworkscm-server.zip) is available for you to use; be sure to replace example values with your own\. You can use the AWS CloudFormation template designer to build a new template or validate an existing one\. For more information about how to do this, see [AWS CloudFormation Designer Interface Overview](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer-overview.html) in the *AWS CloudFormation User Guide*\.  
![\[CloudFormation Create stack page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cfn_select_template.png)

1. On the **Specify stack details** page, enter a name for your stack\. This won't be the same as the name of your server, it is only a stack name\. In the **Parameters** area, paste the values that you created in [Prerequisites](#opscm-create-server-cfn-prereqs)\. Enter the password in **Password**\.

   Paste the contents of the RSA key file in **PivotalKey**\. In the AWS CloudFormation console, you must add newline \(**\\n**\) characters at the end of each line of the pivotal key value, as shown in the following screenshot\. Choose **Next**\.  
![\[Specify stack details page in CloudFormation\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cfn_template_params_opscm.png)

1. On the **Configure stack options** page, you can add tags to the server you're creating with the stack, and choose an IAM role for creating resources if you have not already specified an IAM role to use in your template\. When you're finished specifying options, choose **Next**\. For more information about advanced options such as rollback triggers, see [Setting AWS CloudFormation Stack Options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html) in the *AWS CloudFormation User Guide*\.

1. On the **Review** page, review your choices\. When you are ready to create the server stack, choose **Create stack**\.

   While you are waiting for AWS CloudFormation to create the stack, view the stack creation status\. If stack creation fails, review the error messages shown in the console to help you resolve the issues\. For more information about troubleshooting errors in AWS CloudFormation stacks, see [Troubleshooting Errors](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html#troubleshooting-errors) in the *AWS CloudFormation User Guide*\.

   When server creation is finished, your Chef Automate server is available on the AWS OpsWorks for Chef Automate home page, with a status of **online**\. Generate a new Starter Kit and the Chef Automate dashboard credentials from the server's Properties page\. After the server is online, the Chef Automate dashboard is available on the server's domain, at a URL in the following format: `https://your_server_name-randomID.region.opsworks-cm.io`\.

### Create a Chef Automate Server by using AWS CloudFormation \(CLI\)<a name="opscm-create-server-cfn-cli"></a>

If your local computer is not already running the AWS CLI, download and install the AWS CLI by following [installation instructions](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\. This section does not describe all parameters that you can use with the create\-stack command\. For more information about create\-stack parameters, see [https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html) in the *AWS CLI Reference*\.

1. Be sure to complete the [Prerequisites](gettingstarted-opscm.md#gettingstarted-opscm-prereq) for creating an AWS OpsWorks for Chef Automate server\.

1. Create a service role and an instance profile\. AWS OpsWorks provides an AWS CloudFormation template that you can use to create both\. Run the following AWS CLI command to create an AWS CloudFormation stack that creates the service role and instance profile for you\.

   ```
   aws cloudformation create-stack --stack-name OpsWorksCMRoles --template-url https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-roles.yaml --capabilities CAPABILITY_NAMED_IAM
   ```

   After AWS CloudFormation finishes creating the stack, find and copy the ARNs of service roles in your account\.

   ```
   aws iam list-roles --path-prefix "/service-role/" --no-paginate
   ```

   In the results of the `list-roles` command, look for service role and instance profile entries that resemble the following\. Make a note of the ARNs of the service role and instance profile, and add them to the AWS CloudFormation template that you are using to create your server stack\.

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

1. Create the AWS OpsWorks for Chef Automate server by running the create\-stack command again\.
   + Replace *stack\_name* with the name of your stack\. This is the name of the AWS CloudFormation stack, not your Chef Automate server\. The Chef Automate server name is the value of `ServerName` in your AWS CloudFormation template\.
   + Replace *template* with the path to your template file, and the extension *yaml or json* with `.yaml` or `.json` as appropriate\.
   + The values for `--parameters` correspond to [EngineAttributes](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-opsworkscm-server.html#cfn-opsworkscm-server-engineattributes) from the [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) API\. For Chef, the user\-provided engine attributes to create a server are `CHEF_AUTOMATE_PIVOTAL_KEY`, a base64\-encoded RSA public key that you generate by using utilities described in [Prerequisites](#opscm-create-server-cfn-prereqs), and `CHEF_AUTOMATE_ADMIN_PASSWORD`, a password of between eight and 32 characters that you create\. For more information about the `CHEF_AUTOMATE_ADMIN_PASSWORD`, see [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli)\. You can provide a pointer to the PEM file that contains your pivotal key as the value of the `PivotalKey` parameter, as shown in the example\. If the values for `CHEF_AUTOMATE_ADMIN_PASSWORD` and `CHEF_AUTOMATE_PIVOTAL_KEY` are not specified in your template, you must provide the values in your AWS CLI command\.

   ```
   aws cloudformation create-stack --stack-name stack_name --template-body file://template.yaml or json --parameters ParameterKey=PivotalKey,ParameterValue="base64_encoded_RSA_public_key_value"
   ```

   The following is an example that includes sample values for the `CHEF_AUTOMATE_ADMIN_PASSWORD` and `CHEF_AUTOMATE_PIVOTAL_KEY` attributes\. Run a similar command if you did not specify values for these attributes in your AWS CloudFormation template\.

   ```
   aws cloudformation create-stack --stack-name "OpsWorksCMChefServerStack" --template-body file://opsworkscm-server.yaml --parameters ParameterKey=PivotalKey,ParameterValue="$(openssl rsa -in "pivotalKey.pem" -pubout)" ParameterKey=Password,ParameterValue="SuPer\$ecret890"
   ```

1. When the stack creation is finished, open the Properties page for the new server in the AWS OpsWorks for Chef Automate console, and download a starter kit\. Downloading a new starter kit resets the Chef Automate dashboard administrator password\.

1. Download the root certificate authority \(CA\) certificate from the following Amazon S3 bucket location: [https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2016-root.pem](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2016-root.pem)\. Save the certificate file in a secure but convenient location\. This certificate is required to configure `knife.rb` in the next step\.

1. To use `knife` commands on the new server, update Chef `knife.rb` configuration file settings\. An example `knife.rb` file is included with the starter kit\. The following example shows how to set up `knife.rb`\.
   + Replace *ENDPOINT* with the server's endpoint value\. This is part of the output of the stack creation operation\. You can get the endpoint by running the following command\.

     ```
     aws cloudformation describe-stacks --stack-name stack_name 
     ```
   + Replace *key\_pair\_file\.pem* in the `client_key` configuration with the name of the PEM file that contains the `CHEF_AUTOMATE_PIVOTAL_KEY` that you used to create your server\.

     ```
     base_dir = File.join(File.dirname(File.expand_path(__FILE__)), '..')
     
     log_level                :info
     log_location             STDOUT
     node_name                'pivotal'
     client_key               File.join(base_dir, '.chef', 'key_pair_file.pem')
     syntax_check_cache_path  File.join(base_dir, '.chef', 'syntax_check_cache')
     cookbook_path            [File.join(base_dir, 'cookbooks')]
     
     chef_server_url          'ENDPOINT/organizations/default'
     ssl_ca_file              File.join(base_dir, '.chef', 'ca_certs', 'opsworks-cm-ca-2016-root.pem')
     trusted_certs_dir        File.join(base_dir, '.chef', 'ca_certs')
     ```

1. When the server creation process is finished, go on to [Configure the Chef Server Using the Starter Kit](opscm-starterkit.md)\. If stack creation fails, review the error messages shown in the console to help you resolve the issues\. For more information about troubleshooting errors in AWS CloudFormation stacks, see [Troubleshooting Errors](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html#troubleshooting-errors) in the *AWS CloudFormation User Guide*\.