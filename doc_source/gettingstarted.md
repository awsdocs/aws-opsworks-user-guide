# Getting Started with Chef 11 Linux Stacks<a name="gettingstarted"></a>

**Note**  
This section describes how to get started with Linux stacks using Chef 11\. For information about getting started with Chef 12 Linux stacks, see [Getting Started: Linux](gettingstarted-linux.md)\. For information about getting started with Chef 12 Windows stacks, see [Getting Started: Windows](gettingstarted-windows.md)\.

Cloud\-based applications usually require a group of related resources—application servers, database servers, and so on—that must be created and managed collectively\. This collection of instances is called a *stack*\. A simple application stack might look something like the following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch.png)

The basic architecture consists of the following:
+ A load balancer to distribute incoming traffic from users evenly across the application servers\.
+ A set of application server instances, as many as needed to handle the traffic\.
+ A database server to provide the application servers with a back\-end data store\.

In addition, you typically need a way to distribute applications to the application servers, monitor the stack, and so on\.

AWS OpsWorks Stacks provides a simple and straightforward way to create and manage stacks and their associated applications and resources\. This chapter introduces the basics of AWS OpsWorks Stacks—along with some of its more sophisticated features—by walking you through the process of creating the application server stack in the diagram\. It uses an incremental development model that AWS OpsWorks Stacks makes easy to follow: Set up a basic stack and, once it's working correctly, add components until you arrive at a full\-featured implementation\.
+ [Step 1: Complete the Prerequisites](gettingstarted-prerequisites.md) shows how to get set up to start the walkthrough\.
+ [Step 2: Create a Simple Application Server Stack \- Chef 11](gettingstarted-simple.md) shows how to create a minimal stack that consists of a single application server\.
+ [Step 3: Add a Back\-end Data Store](gettingstarted-db.md) shows how to add a database server and connect it to the application server\.
+ [Step 4: Scale Out MyStack](gettingstarted-scale.md) shows how to scale out a stack to handle increased load by adding more application servers, and a load balancer to distribute incoming traffic\.

**Topics**
+ [Step 1: Complete the Prerequisites](gettingstarted-prerequisites.md)
+ [Step 2: Create a Simple Application Server Stack \- Chef 11](gettingstarted-simple.md)
+ [Step 3: Add a Back\-end Data Store](gettingstarted-db.md)
+ [Step 4: Scale Out MyStack](gettingstarted-scale.md)
+ [Step 5: Delete MyStack](gettingstarted-delete.md)