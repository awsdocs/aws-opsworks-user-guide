# Getting Started with Windows Stacks<a name="gettingstarted-windows"></a>

Cloud\-based applications usually require a group of related resources—application servers, database servers, and so on—that must be created and managed collectively\. This collection of instances is called a *stack*\. A simple application stack might look something like the following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/windows_walkthrough_arch.png)

The basic architecture consists of the following:
+ An Elastic IP address to receive user requests\.
+ A load balancer to distribute incoming requests evenly across the application servers\.
+ A set of application server instances, as many as needed to handle the traffic\.

In addition, you typically need a way to distribute applications to the application servers, manage user permissions, and so on\.

AWS OpsWorks Stacks provides a simple and straightforward way to create and manage stacks and their associated applications and resources\. This chapter introduces the basics of AWS OpsWorks Stacks—along with some of its more sophisticated features—by walking you through the process of creating the application server stack in the diagram\. It uses an incremental development model that AWS OpsWorks Stacks makes easy to follow: Set up a basic stack and, after it's working correctly, add components until you arrive at a full\-featured implementation\.
+ [Step 1: Complete the Prerequisites](gettingstarted-windows-prerequisites.md) shows how to get set up to start the walkthrough\.
+ [Step 2: Create a Basic Application Server Stack](gettingstarted-windows-basic.md) shows how to create a basic stack to support Internet Information Server \(IIS\) and deploy an app to the server\.
+ [Step 3: Scale Out IISExample](gettingstarted-windows-scale.md) shows how to scale out a stack to handle increased load by adding more application servers, a load balancer to distribute incoming traffic, and an Elastic IP address to receive incoming requests\.

**Topics**
+ [Step 1: Complete the Prerequisites](gettingstarted-windows-prerequisites.md)
+ [Step 2: Create a Basic Application Server Stack](gettingstarted-windows-basic.md)
+ [Step 3: Scale Out IISExample](gettingstarted-windows-scale.md)
+ [Next Steps](gettingstarted-windows-what-next.md)