# Step 2: Create a Stack<a name="gettingstarted-intro-create-stack"></a>

In this step, you use the AWS OpsWorks Stacks console to create a stack\. A *stack* is a collection of instances \(such as Amazon EC2 instances\) and related AWS resources that have a common purpose and that you want to manage together\. \(For more information, see [Stacks](workingstacks.md)\.\) There will be only one instance for this walkthrough\.

Before you begin this step, complete the [prerequisites](gettingstarted-intro-prerequisites.md)\.

**To create the stack**

1. Using your IAM user, sign in to the AWS OpsWorks Stacks console at [https://console\.aws\.amazon\.com/opsworks](https://console.aws.amazon.com/opsworks)\.

1. Do any of the following, if they apply:
   + If the **Welcome to AWS OpsWorks Stacks** page is displayed, choose **Add your first stack** or **Add your first AWS OpsWorks Stacks stack** \(both choices do the same thing\)\. The **Add stack** page displays\.
   + If the **OpsWorks Dashboard** page is displayed, choose **Add stack**\. The **Add stack** page displays\.

1. With the **Add stack** page displayed, choose **Sample stack**, if it is not already chosen for you\.

1. With **Linux** already chosen for **Operating system type**, choose **Create stack**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-add-stack-console.png)

1. AWS OpsWorks Stacks creates a stack named **My Sample Stack \(Linux\)**\. AWS OpsWorks Stacks also adds all of the necessary components to deploy the app to the stack:
   + A *layer*, which is a blueprint for a set of instances\. It specifies things like the instance's settings, resources, installed packages, and security groups\. \(For more information, see [Layers](workinglayers.md)\.\) The layer is named **Node\.js App Server**\.
   + An *instance*, which in this case is an Amazon Linux 2 EC2 instance\. \(For more information about instances, see [Instances](workinginstances.md)\.\) The instance's hostname is **nodejs\-server1**\.
   + An *app*, which is code to run on the instance\. \(For more information about apps, see [Apps](workingapps.md)\.\) The app is named **Node\.js Sample App**\.

1. After AWS OpsWorks Stacks creates the stack, choose **Explore the sample stack** to display the **My Sample Stack \(Linux\)** page \(if you complete this walkthrough multiple times, **My Sample Stack \(Linux\)** may have a sequential number after it, such as **2** or **3**\):  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-add-stack-explore-console.png)

In the [next step](gettingstarted-intro-start-instance.md), you will start the instance and deploy the app to the instance\.