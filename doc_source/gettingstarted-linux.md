# Getting Started with Linux Stacks<a name="gettingstarted-linux"></a>

In this walkthrough, you will learn how to use AWS OpsWorks Stacks to create a Node\.js application environment\. When you are done, you will have an Amazon Elastic Compute Cloud \(Amazon EC2\) instance running Chef 12, a Node\.js HTTP server, and a web app that you can use to interact with Twitter and leave comments on a web page\.

Chef is a third\-party framework for configuring and maintaining servers, such as EC2 instances, and how apps are deployed and maintained on those servers\. If you aren't familiar with Chef, after completing this walkthrough, we recommend that you learn more about Chef so that you can take full advantage of all that AWS OpsWorks Stacks has to offer\. \(For more information, see the [Learn Chef](https://learn.chef.io/) website\.\)

AWS OpsWorks Stacks supports four Linux distributions: Amazon Linux, Ubuntu Server, CentOS, and Red Hat Enterprise Linux\. For this walkthrough, we use Ubuntu Server\. AWS OpsWorks Stacks also works with Windows Server\. Although we have an equivalent walkthrough for Windows Server stacks, we recommend that you complete this walkthrough first to learn basic concepts about AWS OpsWorks Stacks and Chef that are not repeated there\. After you complete this walkthrough, see the [Getting Started: Windows](gettingstarted-windows.md) walkthrough\.

**Topics**
+ [Step 1: Complete the Prerequisites](gettingstarted-linux-prerequisites.md)
+ [Step 2: Create a Stack](gettingstarted-linux-create-stack.md)
+ [Step 3: Add a Layer to the Stack](gettingstarted-linux-add-layer.md)
+ [Step 4: Specify the App to Deploy to the Instance](gettingstarted-linux-specify-app.md)
+ [Step 5: Launch an Instance](gettingstarted-linux-launch-instance.md)
+ [Step 6: Deploy the App to the Instance](gettingstarted-linux-deploy-app.md)
+ [Step 7: Test the Deployed App on the Instance](gettingstarted-linux-test-app.md)
+ [Step 8 \(Optional\): Clean Up](gettingstarted-linux-clean-up.md)
+ [Next Steps](gettingstarted-linux-next-steps.md)
+ [Learning More: Explore the Cookbook Used in This Walkthrough](gettingstarted-linux-explore-cookbook.md)
+ [Learning More: Explore the App Used in This Walkthrough](gettingstarted-linux-explore-app-source.md)