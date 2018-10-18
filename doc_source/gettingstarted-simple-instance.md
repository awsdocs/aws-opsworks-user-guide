# Step 2\.3: Add an Instance to the PHP App Server Layer \- Chef 11<a name="gettingstarted-simple-instance"></a>

An AWS OpsWorks Stacks instance represents a particular Amazon EC2 instance:
+ The instance's configuration specifies some basics like the Amazon EC2operating system and size; it runs but doesn't do very much\. 
+ The instance's layer adds functionality to the instance by determining which packages are to be installed, whether the instance has an Elastic IP address, and so on\.

AWS OpsWorks Stacks installs an agent on each instance that interacts with the service\. To add a layer's functionality to an instance, AWS OpsWorks Stacks directs the agent to run small applications called [Chef recipes](http://docs.chef.io/recipes.html), which can install applications and packages, create configuration files, and so on\. AWS OpsWorks Stacks runs recipes at key points in the instance's [lifecycle](workingcookbook-events.md)\. For example, OpsWorks runs Setup recipes after the instance has finished booting to handle tasks such as installing software, and runs Deploy recipes when you deploy an app to install the code and related files\.

**Note**  
If you are curious about how the recipes work, all of the AWS OpsWorks Stacks built\-in recipes are in a public GitHub repository: [OpsWorks Cookbooks](https://github.com/aws/opsworks-cookbooks)\. You can also create your own custom recipes and have AWS OpsWorks Stacks run them, as described later\.

To add a PHP application server to MyStack, add an instance to the PHP App Server layer that you created in the previous step\. 

**To add an instance to the PHP App Server layer**

1. 

**Open Add an Instance**

   After you finish adding the layer, AWS OpsWorks Stacks displays the **Layers** page\. Click **Instances** in the navigation pane and under **PHP App Server**, click **Add an instance**\.

1. 

**Configure the Instance**

   Each instance has a default host name that is generated for you by AWS OpsWorks Stacks\. In this example, AWS OpsWorks Stacks simply adds a number to the layer's short name\. You can configure each instance separately, including overriding some of the default settings that you specified when creating the stack, such as the Availability Zone or operating system\. For this walkthrough, just accept the default settings and click **Add Instance** to add the instance to the layer\. For more information, see [Instances](workinginstances.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs7.png)

1. 

**Start the Instance**

   So far, you have just specified the instance's configuration\. You have to start an instance to create a running Amazon EC2 instance\. AWS OpsWorks Stacks then uses the configuration settings to launch an Amazon EC2 instance in the specified Availability Zone\. The details of how you start an instance depend on the instance's *scaling type*\. In the previous step, you created an instance with the default scaling type, *24/7*, which must be manually started and then runs until it is manually stopped\. You can also create time\-based and load\-based scaling types, which AWS OpsWorks Stacks automatically starts and stops based on a schedule or the current load\. For more information, see [Managing Load with Time\-based and Load\-based Instances](workinginstances-autoscaling.md)\.

   Go to **php\-app1** under **PHP App Server** and click **start** in the row's **Actions** column to start the instance\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs8.png)

1. 

**Monitor the Instance's Status during Startup**

   It typically takes a few minutes to boot the Amazon EC2 instance and install the packages\. As startup progresses, the instance's **Status** field displays the following series of values: 

   1. **requested** \- AWS OpsWorks Stacks has called the Amazon EC2 service to create the Amazon EC2 instance\.

   1. **pending** \- AWS OpsWorks Stacks is waiting for the Amazon EC2 instance to start\.

   1. **booting** \- The Amazon EC2 instance is booting\.

   1. **running\_setup** \- The AWS OpsWorks Stacks agent is running the layer's Setup recipes, which handle tasks such as configuring and installing packages, and the Deploy recipes, which deploy any apps to the instance\.

   1. **online** \- The instance is ready for use\.

   After php\-app1 comes online, the **Instances** page should look like the following:   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs9.png)

   The page begins with a quick summary of all your stack's instances\. Right now, it shows one online instance\. In the php\-app1 **Actions** column, notice that **stop**, which stops the instance, has replaced **start** and **delete**\.