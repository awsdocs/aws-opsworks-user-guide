# Step 2\.1: Create the Stack<a name="gettingstarted-windows-stack"></a>

You start an AWS OpsWorks Stacks project by creating a stack, which acts as a container for your instances and other resources\. The stack configuration specifies some basic settings, such as the AWS region and the default operating system, that are shared by all the stack's instances\. 

**To create a new stack**

1. 

**Add a Stack**

   If you haven't done so already, sign in to the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/)\.
   + If the account has no existing stacks, you will see the **Welcome to AWS OpsWorks** page; choose **Add your first stack**\.
   + Otherwise, you will see the AWS OpsWorks Stacks dashboard, which lists your account's stacks; choose **Add Stack**\.

1. 

**Configure the Stack**

   On the **Add Stack** page, choose **Chef 12 stack** and specify the following settings:  
**Stack name**  
Enter a name for your stack, which can contain alphanumeric characters \(a–z, A–Z, and 0–9\), and hyphens \(\-\)\. The example stack for this walkthrough is named **IISWalkthrough**\.  
**Region**  
Select US West \(Oregon\) as the stack's region\.  
You can create a stack in any region, but we recommend US West \(Oregon\) for tutorials\.  
**Default operating system**  
Choose **Windows**, and then specify **Microsoft Windows Server 2012 R2 Base**, which is the default setting\.  
**Use custom Chef cookbooks**  
For the purposes of this walkthrough, specify **No** for this option\.

1. Choose **Advanced** to confirm that you have an IAM role and the default IAM instance profile selected\.  
IAM role  
Specify the stack's IAM \(AWS Identity and Access Management\) role\. AWS OpsWorks Stacks needs to access other AWS services to perform tasks such as creating and managing Amazon EC2 instances\. **IAM role** specifies the role that AWS OpsWorks Stacks assumes to work with other AWS services on your behalf\. For more information, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\.  
   + If your account has an existing AWS OpsWorks Stacks IAM role, you can select it from the list\.

     If the role was created by AWS OpsWorks Stacks, it will be named `aws-opsworks-service-role`\.
   + Otherwise, select **New IAM Role** to direct AWS OpsWorks Stacks to create a new role for you with the correct permissions\. 

     **Note:** If you have AWS OpsWorks Stacks Full Access permissions, creating a new role requires several additional IAM permissions\. For more information, see [Example Policies](opsworks-security-users-examples.md)\.

1. Accept the default values for the other settings and choose **Add Stack**\. For more information on the various stack settings, see [Create a New Stack](workingstacks-creating.md)\.