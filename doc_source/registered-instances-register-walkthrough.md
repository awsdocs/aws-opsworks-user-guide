# Walkthrough: Register an Instance from Your Workstation<a name="registered-instances-register-walkthrough"></a>

**Note**  
This feature is supported only for Linux stacks\.

The registration process supports several scenarios\. This section walks you through an end\-to\-end example of one scenario: how to use your workstation to register an Amazon EC2 instance\. Other registration scenarios use a similar procedure\. For more information, see [Registering Amazon EC2 and On\-premises Instances](registered-instances-register-registering.md)\.

**Note**  
You typically want to register an existing Amazon EC2 instance\. However, you can just create a new instance and a new stack for the walkthrough and delete them when you are finished\.

**Topics**
+ [Step 1: Create a Stack and an Instance](#registered-instances-register-walkthrough-prepare)
+ [Step 2: Install and Configure the AWS CLI](#registered-instances-register-walkthrough-cli)
+ [Step 3: Register the Instance with the EC2Register Stack](#registered-instances-register-walkthrough-register)

## Step 1: Create a Stack and an Instance<a name="registered-instances-register-walkthrough-prepare"></a>

To get started, you need a stack and an Amazon EC2 instance to be registered with that stack\.

**To create the stack and instance**

1. Use the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) to [create a new stack](workingstacks-creating.md) named **EC2Register**\. You can accept default values for the other stack settings\.

1. Launch a new instance from the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\. Note the following\.
   + The instance must in the same region and VPC as the stack\.

     If you are using a VPC, pick a public subnet for this walkthrough\.
   + If you need to create an SSH key, save the private key file to your workstation and record the name and file location\.

     If you use an existing key, record the name and private key file location\. You need those values later\.
   + The instance must be based on one of the [supported Linux operating systems](workinginstances-os-linux.md)\. For example, if your stack is in US West \(Oregon\), you can use `ami-35501205` to launch a Ubuntu 14\.04 LTS instance in that region\.

   Otherwise, accept the default values\.

While the instance is booting, you can proceed to the next section\.

## Step 2: Install and Configure the AWS CLI<a name="registered-instances-register-walkthrough-cli"></a>

Registration is performed by the `aws opsworks register` AWS CLI command\. Before you register your first instance, you must be running version 1\.16\.180 of the AWS CLI or newer\. The installation details depend on your workstation's operating system\. For more information about installing the AWS CLI, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\. To check the version of the AWS CLI that you are running, enter `aws --version` in a shell session\.

We strongly recommend that you do not skip this step, even if you are already running the AWS CLI on your workstation\. Using the most current release of the AWS CLI is a security best practice\.

You must provide `register` with a set of AWS credentials that have appropriate permissions\. The recommended way to do this—so that you avoid installing credentials directly on an instance—is to register instances that are launched with an instance profile, and then add the `--use-instance-profile` switch to your `register` command\. If you are getting credentials from an instance profile, skip to [Step 3: Register the Instance with the EC2Register Stack](#registered-instances-register-walkthrough-register) in this topic\. However, if your instance was not launched with an instance profile, you can create an IAM user\. The following procedure creates a new IAM user with appropriate permissions, installing the user's credentials on the workstation, and then passing those credentials to `register`\.

**To create the IAM User**

1. On the [IAM console](https://console.aws.amazon.com/iam/), choose **Users** in the navigation pane, and then choose **Add user**\.

1. Add a user named **EC2Register**\. In the **Select AWS access type** area, select **Programmatic access**, and then choose **Next: Permissions**\.

1. On the **Set permissions** page, choose **Attach existing policies directly**\.

1. Enter **OpsWorks** in the **Policy type** filter box to display the AWS OpsWorks policies, select one of the following policies, and then choose **Next: review**\. The policy grants your user the permissions that are required to run `register`\.
   + Choose `AWSOpsWorksRegisterCLI_EC2` to allow the user permissions to register EC2 instances that use instance profiles\.
   + Choose `AWSOpsWorksRegisterCLI_OnPremises` to allow the user permissions to register on\-premises instances\.

1. On the **Review** page, choose **Create user**\.

1. Choose **Download \.csv**, save the credentials file to a convenient location on your system, and then choose **Close**\.

You need to provide the IAM user's credentials to `register`\. This walkthrough handles the task by installing the EC2Register credentials in your workstation's `credentials` file\. For information about other ways to manage credentials for the AWS CLI, see [Configuration and Credential Files](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files)\.

**To install the user's credentials**

1. Create or open your workstation's `credentials` file\. The file is located at `~/.aws/credentials` \(Linux, Unix, and OS X\) or `C:\Users\User_Name\.aws\credentials` \(Windows systems\)\.

1. Add a profile for the EC2Register user to the `credentials` file, using the following format\.

   ```
   [ec2register]
   aws_access_key_id = access_key_id
   aws_secret_access_key = secret_access_key
   ```

   Replace *access\_key\_id* and *secret\_access\_key* with the EC2Register keys for that you downloaded earlier\.

## Step 3: Register the Instance with the EC2Register Stack<a name="registered-instances-register-walkthrough-register"></a>

You are now ready to register the instance\.

**To register the instance**

1. In AWS OpsWorks Stacks, return to the EC2Register stack, choose **Instances** in the navigation pane, and then choose **Register an instance**\.

1. Select **EC2 Instances**, choose **Next: Select Instances**, and select your instance from the list\.

1. Choose **Next: Install AWS CLI**, and **Next: Register Instances**\. AWS OpsWorks Stacks automatically uses the available information, such as the stack ID and the instance ID to create a `register` command template, which is displayed on the **Register Instances** page\. For this example, you use `register` to log in to the instance with an SSH key and explicitly specify the key file, so set **I use SSH keys to connect to my instances** to **Yes**\. The command template resembles the following\.

   ```
   aws opsworks register --infrastructure-class ec2 --region region endpoint ID
     --stack-id 247be7ea-3551-4177-9524-1ff804f453e3 --ssh-username [username]
     --ssh-private-key [key-file] i-f1245d10
   ```
**Note**  
You must set the region to the AWS OpsWorks Stacks service's endpoint region, not the stack's region, if the stack is within a classic region associated with the `us-east-1` regional endpoint\. AWS OpsWorks Stacks determines the stack's region from the stack ID\.

1. The command template contains several user\-specific argument values, which are indicated by brackets and must be replaced with appropriate values\. Copy the command template to a text editor and edit it as follows\.
**Important**  
The IAM user that is created during the registration process is required throughout the life of a registered instance\. Deleting the user causes the AWS OpsWorks Stacks agent to be unable to communicate with the service\. To help prevent problems managing registered instances in the event that the IAM user is accidentally deleted, add the `--use-instance-profile` parameter to your `register` command to use the instance's built\-in instance profile instead\. Adding the `--use-instance-profile` parameter also prevents errors from occurring when you rotate AWS account access keys every 90 days \(a recommended best practice\), because it prevents mismatches between the access keys available to the AWS OpsWorks agent and required IAM user\.
   + Replace *key file* with the fully qualified path of the private key file for the Amazon EC2 key pair that you saved when you created the instance\.

     You can use a relative path, if you prefer\.
   + Replace *username* with the instance's user name\.

     For this example, the user name is either `ubuntu`, for an Ubuntu instance, or `ec2-user`, for a Red Hat Enterprise Linux \(RHEL\) or Amazon Linux instance\.
   + Add `--use-instance-profile`, which runs `register` with the instance profile to prevent errors during key rotation or if the principal IAM user is accidentally deleted\.

   Your command should resemble the following\.

   ```
   aws opsworks register --use-instance-profile --infrastructure-class ec2 \
     --region us-west-2  --stack-id 247be7ea-3551-4177-9524-1ff804f453e3 --ssh-username ubuntu \
     --ssh-private-key "./keys/mykeys.pem" i-f1245d10
   ```

1. Open a terminal window on your workstation, paste the `register` command from your editor, and run the command\. 

   Registration typically takes around five minutes\. When it is complete, return to the AWS OpsWorks Stacks console and choose **Done**\. Then choose **Instances** in the navigation pane\. Your instance should be listed under **Unassigned Instances**\. You can then [assign the instance to a layer](registered-instances-assign.md) or leave it where it is, depending on how you intend to manage the instance\.

1. When you are finished, [stop the instance](workinginstances-starting.md#workinginstances-starting-stop) and then [delete it](workinginstances-starting.md#workinginstances-starting-stop) by using the AWS OpsWorks Stacks console or commands\. This terminates the Amazon EC2 instance, so you don't incur any further charges\.