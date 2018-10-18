# Step 2\.4: Create and Deploy an App \- Chef 11<a name="gettingstarted-simple-app"></a>

To make MyStack more useful, you need to deploy an app to the PHP App Server instance\. You store an app's code and any related files in a repository, such as Git\. You need to take a couple of steps to get those files to your application servers:

**Note**  
The procedure in this section applies to Chef 11 stacks\. For information about how to add apps to layers in Chef 12 stacks, see [Adding Apps](workingapps-creating.md)\.

1. Create an app\.

   An app contains the information that AWS OpsWorks Stacks needs in order to download the code and related files from the repository\. You can also specify additional information such as the app's domain\.

1. Deploy the app to your application servers\.

   When you deploy an app, AWS OpsWorks Stacks triggers a Deploy lifecycle event\. The agent then runs the instance's Deploy recipes, which download the files to the appropriate directory along with related tasks such as configuring the server, restarting the service, and so on\.

**Note**  
When you create a new instance, AWS OpsWorks Stacks automatically deploys any existing apps to the instance\. However, when you create a new app or update an existing one, you must manually deploy the app or update to all existing instances\.

This step shows how to manually deploy an example app from a public Git repository to an application server\. If you would like to examine the application, go to [https://github\.com/amazonwebservices/opsworks\-demo\-php\-simple\-app](https://github.com/amazonwebservices/opsworks-demo-php-simple-app)\. The application used in this example is in the version1 branch\. AWS OpsWorks Stacks also supports several other repository types\. For more information, see [Application Source](workingapps-creating.md#workingapps-creating-source)\. 

**To create and deploy an app**

1. 

**Open the Apps Page**

   In the navigation pane, click **Apps** and on the **Apps** page, click **Add an app**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs13.png)

1. 

**Configure the App**

   On the **App** page, specify the following values:  
**Name**  
The app's name, which AWS OpsWorks Stacks uses for display purposes\. The example app is named **SimplePHPApp**\. AWS OpsWorks Stacks also generates a short name—simplephpapp for this example—that is used internally and by the Deploy recipes, as described later\.  
**Type**  
The app's type, which determines where to deploy the app\. The example uses **PHP**, which deploys the app to PHP App Server instances\.  
**Data source type**  
An associated database server\. For now, select **None**; we'll introduce database servers in [Step 3: Add a Back\-end Data Store](gettingstarted-db.md)\.  
**Repository type**  
The app's repository type\. The example app is stored in a **Git** repository\.   
**Repository URL**  
The app's repository URL\. The example URL is: **git://github\.com/awslabs/opsworks\-demo\-php\-simple\-app\.git**  
**Branch/Revision**  
The app's branch or version\. This part of the walkthrough uses the **version1** branch\.

   Keep the default values for the remaining settings and click **Add App**\. For more information, see [Adding Apps](workingapps-creating.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs14.png)

1. 

**Open the Deployment Page**

   To install the code on the server, you must *deploy* the app\. To do so, click **deploy** in the SimplePHPApp **Actions** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs15.png)

1. 

**Deploy the App**

   When you deploy an app, the agent runs the Deploy recipes on the PHP App Server instance, which download and configure the application\. 

   **Command** should already be set to **deploy**\. Keep the defaults for the other settings and click **Deploy** to deploy the app\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs16.png)

   When deployment is complete, the **Deployment** page displays a **Status** of **Successful**, and **php\-app1** will have a green check mark next to it\.

1. 

**Run SimplePHPApp**

   SimplePHPApp is now installed and ready to go\. To run it, click **Instances** in the navigation pane to go to the **Instances** page\. Then click the php\-app1 instance's public IP address\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs20.png)

   You should see a page such as the following in your browser\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs21.png)

**Note**  
This walkthrough assumes that you will go on to the next section and ultimately complete the entire walkthrough in one session\. If you prefer, you can stop at any point and continue later by signing in to AWS OpsWorks Stacks and opening the stack\. However, you are charged for any AWS resources that you use, such as online instances\. To avoid unnecessary charges, you can stop your instance, which terminates the corresponding EC2 instance\. You can start the instances again when you are ready to continue\.