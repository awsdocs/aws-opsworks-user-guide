# Registering the Instance<a name="registered-instances-register-registering-register"></a>

**Note**  
This feature is supported only for Linux stacks\.

You register an instance by running the AWS CLI `register` command from your workstation or from the instance\. The simplest way to handle the operation is to use the [AWS OpsWorks Stacks console's](https://console.aws.amazon.com/opsworks/) registration wizard, which simplifies the process of constructing the command string\. After you are familiar with the registration procedure, you can skip the wizard if you prefer, and run the `register` command\.

The following describes how to use the registration wizard to register an instance with an existing stack\.

**Note**  
To register an instance with a new stack, you can do so by choosing **Register Instances** on the AWS OpsWorks Stacks dashboard\. This starts a wizard that is identical to the one for existing stacks, except for an additional page that configures the new stack\.

**To use the registration wizard to register an instance**

1. In the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/), create a stack or open an existing stack\.

1. Choose **Instances** in the navigation pane, and then choose **register an instance**\.

1. On the **Choose an Instance Type** page, specify whether you want to register an Amazon EC2 or an on\-premises instance:
   + If you are registering an Amazon EC2 instance, choose **Next: Select Instances**\.
   + If you are registering an on\-premises instance, choose **Next: Install AWS CLI**, and then go to Step 5\.

1. If you are registering an Amazon EC2 instance, open the **Select Instances** page to select the instance to register\. AWS OpsWorks Stacks collects the information needed to build the command\. When you are finished, choose **Next:Install AWS CLI**\.

1. The instance on which you plan to run `register` must be running version 1\.16\.180 of the AWS CLI or newer\. To install or update the AWS CLI, the registration wizard page provides links to installation and configuration instructions\. After you have verified the AWS CLI installation, choose whether you are running the command from the instance to be registered or from a separate workstation, and then choose **Next: Register Instances**\.

1. The **Register Instances** page displays a template for a `register` command string that incorporates your selected options\. For example, if you are registering an Amazon EC2 instance from a separate workstation, the default template resembles the following\.

   ```
   aws opsworks register --infrastructure-class ec2 --region us-west-2
     --stack-id 247be7ea-3551-4177-9524-1ff804f453e3 --ssh-username [username] i-f1245d10
   ```
**Important**  
The IAM user that is created during the registration process is required throughout the life of a registered instance\. Deleting the user causes the AWS OpsWorks Stacks agent to be unable to communicate with the service\. To help prevent problems managing registered instances in the event that the IAM user is accidentally deleted, add the `--use-instance-profile` parameter to your `register` command to use the instance's built\-in instance profile instead\. Adding the `--use-instance-profile` parameter also prevents errors from occurring when you rotate AWS account access keys every 90 days \(a recommended best practice\), because it prevents mismatches between the access keys available to the AWS OpsWorks agent and required IAM user\.

   If you set **I use SSH keys** to **Yes**, AWS OpsWorks Stacks adds the `--ssh-private-key` argument to the string, which you can use to specify a private SSH key file\.
**Note**  
If you want `register` to log on with a password, set **I use SSH keys** to **No**\. When you run `register`, you are prompted for the password\.

   Copy this string to a text editor\. and edit it as required\. Note the following\.
   + The bracketed text represents information that you must supply, such as the location of your SSH key file\.
   + The template assumes that you are running `register` with default AWS credentials\. If not, add a `--profile` argument to the command string, and specify the credential profile name that you want to use\.

   For other scenarios, you might need to change the command further\. For an explanation of the available `register` arguments and alternative ways to construct the command string, see [Using the `register` Command](registered-instances-register-registering-command.md)\. You can also display the command's documentation by running `aws opsworks help register` from the command line\. For some example command strings, see [Example register Commands](registered-instances-register-registering-examples.md)\.

1. After you have finished editing the command string, open a terminal window on your workstation or use SSH to log on to the instance and run the command\. The entire operation typically takes around five minutes, during which the instance is in the **Registering** state\.

1. When the operation is finished, choose **Done**\. The instance is now in the **Registered** state and is listed as an unassigned instance on the stack's **Instances** page\.

The `register` command does the following\.

1. If `register` is running on a workstation, the command first uses SSH to log in to the instance to be registered\.

   The remainder of the process takes place on the instance, and is the same regardless of where you ran the command\.

1. Downloads the AWS OpsWorks Stacks agent package from Amazon S3\.

1. Unpacks and installs the agent and its dependencies, such as the [AWS SDK for Ruby](http://aws.amazon.com/documentation/sdk-for-ruby/)\.

1. Creates the following:
   + An IAM user that bootstraps the agent with the AWS OpsWorks Stacks service to provide secure communication\.

     The user's permissions allow only the `opsworks:RegisterInstance` action, and they expire after 15 minutes\.
   + An IAM group for the stack, which contains the registered instances' IAM users\.

1. Creates an RSA key pair and sends the public key to AWS OpsWorks Stacks\.

   This key pair is used to encrypt communications between the agent and AWS OpsWorks Stacks\.

1. Registers the instance with AWS OpsWorks Stacks\. The stack then runs a set of initial setup recipes to configure the instance, which includes the following\.
   + Overwriting the instance's hosts file\.

     By registering the instance, you have handed user management over to AWS OpsWorks Stacks, which must have its own hosts file to control SSH login permissions\.
   + For Amazon EC2 instances, initial setup also includes registering any attached Amazon EBS volumes or Elastic IP addresses with the stack\.

     You must ensure that the Amazon EBS volumes are not mounted to reserved mount points, including `/var/www` and any mount points that are reserved by the instance's layers\. For more information about managing stack resources, see [Resource Management](resources.md)\. For more information about layer mount points, see [AWS OpsWorks Stacks Layer Reference](layers.md)\.

   For a complete description of the initial setup configuration changes, see [Initial Setup Configuration Changes](registered-instances-lifecycle.md#registered-instances-lifecycle-setup-config)\.
**Note**  
Initial setup does not update a registered instance's operating system; you must handle that task yourself\. For more information, see [Managing Security Updates](workingsecurity-updates.md)\.