# Step 1: Complete the Prerequisites<a name="gettingstarted-intro-prerequisites"></a>

You must complete the following setup steps before you can start the walkthrough\. These setup steps include signing up for an AWS account, creating an IAM user in your AWS account, and assigning the IAM user access permissions to AWS OpsWorks Stacks\.

**Topics**
+ [Step 1\.1: Sign up for an AWS Account](#gettingstarted-intro-prerequisites-aws-account)
+ [Step 1\.2: Create an IAM User in Your AWS Account](#gettingstarted-intro-prerequisites-iam-user)
+ [Step 1\.3: Assign Service Access Permissions to Your IAM User](#gettingstarted-intro-prerequisites-permissions)

## Step 1\.1: Sign up for an AWS Account<a name="gettingstarted-intro-prerequisites-aws-account"></a>

In this step, you will sign up for an AWS account\. If you already have an AWS account that you want to use for this walkthrough, you can skip ahead to [Step 1\.2: Create an IAM User in Your AWS Account](#gettingstarted-intro-prerequisites-iam-user)\. You cannot use an AWS Free Tier account for this walkthrough\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Step 1\.2: Create an IAM User in Your AWS Account<a name="gettingstarted-intro-prerequisites-iam-user"></a>

Use your AWS account to create an AWS Identity and Access Management \(IAM\) user\. You use an IAM user to access the AWS OpsWorks Stacks service\. \(We don't recommend that you use your AWS account to access AWS OpsWorks Stacks directly\. Doing so is generally less secure and can make it more difficult for you to troubleshoot service access issues later\.\) If you already have an IAM user that you want to use for this walkthrough, you can skip ahead to [Step 1\.3: Assign Service Access Permissions to Your IAM User](#gettingstarted-intro-prerequisites-permissions)\.

To create an IAM user, see [Creating IAM Users \(AWS Management Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console)\. For this walkthrough, the user name is `OpsWorksDemoUser`\. However, you can use a different name\.

## Step 1\.3: Assign Service Access Permissions to Your IAM User<a name="gettingstarted-intro-prerequisites-permissions"></a>

Set up your IAM user to enable access to the AWS OpsWorks Stacks service \(and related services that AWS OpsWorks Stacks relies on\)\.

To assign service access permissions to your IAM user, see [Attaching Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console)\. Attach the `AWSOpsWorksFullAccess` and `AmazonS3FullAccess` policies to the user you created in the previous step or to the existing IAM user that you want to use\.

You have now completed all of the setup steps and can [start this walkthrough](gettingstarted-intro-create-stack.md)\.