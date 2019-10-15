# Step 1: Complete the Prerequisites<a name="gettingstarted-windows-prerequisites"></a>

Complete the following setup steps before you can start the walkthrough\. These setup steps include signing up for an AWS account, creating an IAM user in your AWS account, and assigning the IAM user access permissions to AWS OpsWorks Stacks\. 

If you have already completed the [Getting Started: Sample](gettingstarted-intro.md) or [Getting Started: Linux](gettingstarted-linux.md) walkthroughs, then you have met the prerequisites for this walkthrough, and you can skip ahead to [Step 2: Create a Basic Application Server Stack](gettingstarted-windows-basic.md)\.

**Topics**
+ [Step 1\.1: Sign up for an AWS Account](#gettingstarted-windows-prerequisites-aws-account)
+ [Step 1\.2: Create an IAM User in Your AWS Account](#gettingstarted-windows-prerequisites-iam-user)
+ [Step 1\.3: Assign Service Access Permissions to Your IAM User](#gettingstarted-windows-prerequisites-permissions)
+ [Step 1\.4: Ensure AWS OpsWorks Stacks Users are Added to Your Domain](#gettingstarted-windows-prerequisites-adusers)

## Step 1\.1: Sign up for an AWS Account<a name="gettingstarted-windows-prerequisites-aws-account"></a>

In this step, you will sign up for an AWS account\. If you already have an AWS account that you want to use for this walkthrough, you can skip ahead to [Step 1\.2: Create an IAM User in Your AWS Account](#gettingstarted-windows-prerequisites-iam-user)\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Step 1\.2: Create an IAM User in Your AWS Account<a name="gettingstarted-windows-prerequisites-iam-user"></a>

Use your AWS account to create an AWS Identity and Access Management \(IAM\) user\. You use an IAM user to access the AWS OpsWorks Stacks service\. \(We don't recommend that you use your AWS account to access AWS OpsWorks Stacks directly\. Doing so is generally less secure and can make it more difficult for you to troubleshoot service access issues later\.\) If you already have an IAM user that you want to use for this walkthrough, you can skip ahead to [Step 1\.3: Assign Service Access Permissions to Your IAM User](#gettingstarted-windows-prerequisites-permissions)\.

To create an IAM user, see [Creating IAM Users \(AWS Management Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console)\. For this walkthrough, we will refer to the user as `OpsWorksDemoUser`\. However, you can use a different name\.

## Step 1\.3: Assign Service Access Permissions to Your IAM User<a name="gettingstarted-windows-prerequisites-permissions"></a>

Set up your IAM user to enable access to the AWS OpsWorks Stacks service \(and related services that AWS OpsWorks Stacks relies on\)\.

To assign service access permissions to your IAM user, see [Attaching Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console)\. Attach the `AWSOpsWorksFullAccess` and `AmazonS3FullAccess` policies to the user you created in the previous step or to the existing IAM user that you want to use\.

## Step 1\.4: Ensure AWS OpsWorks Stacks Users are Added to Your Domain<a name="gettingstarted-windows-prerequisites-adusers"></a>

In a Chef 12\.2 stack, the included `aws_opsworks_users` cookbook creates users that have SSH and Remote Desktop Protocol \(RDP\) access to Windows\-based instances\. When you join Windows instances in your stack to an Active Directory domain, this cookbook run can fail if the AWS OpsWorks Stacks users do not exist in Active Directory\. If the users are not recognized in Active Directory, instances can enter a `setup failed` state when you restart them after joining them to a domain\. For domain\-joined Windows instances, it is not sufficient to grant AWS OpsWorks Stacks users SSH/RDP access on the user permissions page\.

Before you join Windows instances in a Chef 12\.2 stack to an Active Directory domain, be sure all AWS OpsWorks Stacks users of the Windows\-based stack are members of the domain\. The best way to do this is to configure federated identity with IAM before creating your Windows\-based stack, and then import federated users into AWS OpsWorks Stacks before joining instances in your stack to a domain\. For more information about how to do this, see [Enabling Federation to AWS Using Windows Active Directory, ADFS, and SAML 2\.0](https://aws.amazon.com/blogs/security/enabling-federation-to-aws-using-windows-active-directory-adfs-and-saml-2-0/) in the AWS Security Blog, and [Federating Existing Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html#intro-identity-federation) in the *IAM User Guide*\.