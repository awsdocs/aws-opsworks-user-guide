# Step 1: Complete the Prerequisites<a name="gettingstarted-prerequisites"></a>

Complete the following setup steps before you can start the walkthrough\. These setup steps include signing up for an AWS account, creating an IAM user in your AWS account, and assigning the IAM user access permissions to AWS OpsWorks Stacks\. 

If you have already completed any of the [Getting Started with AWS OpsWorks Stacks](gettingstarted_intro.md) walkthroughs, then you have met the prerequisites for this walkthrough, and you can skip ahead to [Step 2: Create a Simple Application Server Stack \- Chef 11](gettingstarted-simple.md)\.

**Topics**
+ [Step 1\.1: Sign up for an AWS Account](#gettingstarted-prerequisites-aws-account)
+ [Step 1\.2: Create an IAM User in Your AWS Account](#gettingstarted-prerequisites-iam-user)
+ [Step 1\.3: Assign Service Access Permissions to Your IAM User](#gettingstarted-prerequisites-permissions)

## Step 1\.1: Sign up for an AWS Account<a name="gettingstarted-prerequisites-aws-account"></a>

In this step, you will sign up for an AWS account\. If you already have an AWS account that you want to use for this walkthrough, you can skip ahead to [Step 1\.2: Create an IAM User in Your AWS Account](#gettingstarted-prerequisites-iam-user)\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root)\.

## Step 1\.2: Create an IAM User in Your AWS Account<a name="gettingstarted-prerequisites-iam-user"></a>

Use your AWS account to create an AWS Identity and Access Management \(IAM\) user\. You use an IAM user to access the AWS OpsWorks Stacks service\. \(We don't recommend that you use your AWS account to access AWS OpsWorks Stacks directly\. Doing so is generally less secure and can make it more difficult for you to troubleshoot service access issues later\.\) If you already have an IAM user that you want to use for this walkthrough, you can skip ahead to [Step 1\.3: Assign Service Access Permissions to Your IAM User](#gettingstarted-prerequisites-permissions)\.

To create an IAM user, see [Creating IAM Users \(AWS Management Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console)\. For this walkthrough, we will refer to the user as `OpsWorksDemoUser`\. However, you can use a different name\.

## Step 1\.3: Assign Service Access Permissions to Your IAM User<a name="gettingstarted-prerequisites-permissions"></a>

Set up your IAM user to enable access to the AWS OpsWorks Stacks service \(and related services that AWS OpsWorks Stacks relies on\)\.

To assign service access permissions to your IAM user, see [Attaching Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console)\. Attach the `AWSOpsWorks_FullAccess` and `AmazonS3FullAccess` policies to the user you created in the previous step or to the existing IAM user that you want to use\.

You have now completed all of the setup steps and can [start this walkthrough](gettingstarted-simple.md)\.