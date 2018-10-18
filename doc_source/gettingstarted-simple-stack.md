# Step 2\.1: Create a Stack \- Chef 11<a name="gettingstarted-simple-stack"></a>

You start an AWS OpsWorks Stacks project by creating a stack, which acts as a container for your instances and other resources\. The stack configuration specifies some basic settings, such as the AWS region and the default operating system, that are shared by all the stack's instances\.

**Note**  
This page helps you create Chef 11 stacks\. For information about how to create Chef 12 stacks, see [Create a Stack](https://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-intro-create-stack.html)\.

This page helps you create stacks in Chef 11\. 

**To create a new stack**

1. 

**Add a Stack**

   Sign into the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/)\. If the account has no existing stacks, you will see the **Welcome to AWS OpsWorks** page; click **Add your first stack**\. Otherwise, you will see the AWS OpsWorks Stacks dashboard, which lists your account's stacks; click **Add Stack**\.  
![\[If you have no stacks, you'll see the first-run page in the AWS OpsWorks Stacks console; otherwise, you'll see a list of all your account's stacks.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/firstrun.png)

1. 

**Configure the Stack**

   On the **Add Stack** page, choose **Chef 11 stack** and specify the following settings:  
**Stack name**  
Enter a name for your stack, which can contain alphanumeric characters \(a–z, A–Z, and 0–9\), and hyphens \(\-\)\. The example stack for this walkthrough is named **MyStack**\.  
**Region**  
Select US West \(Oregon\) as the stack's region\.

   Accept the default values for the other settings and click **Add Stack**\. For more information on the various stack settings, see [Create a New Stack](workingstacks-creating.md)\.