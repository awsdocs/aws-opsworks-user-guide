# Stacks<a name="workingstacks"></a>

The stack is the top\-level AWS OpsWorks Stacks entity\. It represents a set of instances that you want to manage collectively, typically because they have a common purpose such as serving PHP applications\. In addition to serving as a container, a stack handles tasks that apply to the group of instances as a whole, such as managing applications and cookbooks\.

For example, a stack whose purpose is to serve web applications might look something like the following:
+ A set of application server instances, each of which handles a portion of the incoming traffic\.
+ A load balancer instance, which takes incoming traffic and distributes it across the application servers\.
+ A database instance, which serves as a back\-end data store for the application servers\.

A common practice is to have multiple stacks that represent different environments\. A typical set of stacks consists of:
+ A development stack to be used by developers to add features, fix bugs, and perform other development and maintenance tasks\.
+ A staging stack to verify updates or fixes before exposing them publicly\.
+ A production stack, which is the public\-facing version that handles incoming requests from users\.

This section describes the basics of working with stacks\.

**Topics**
+ [Create a New Stack](workingstacks-creating.md)
+ [Running a Stack in a VPC](workingstacks-vpc.md)
+ [Update a Stack](workingstacks-edit.md)
+ [Clone a Stack](workingstacks-cloning.md)
+ [Run AWS OpsWorks Stacks Stack Commands](workingstacks-commands.md)
+ [Using Custom JSON](workingstacks-json.md)
+ [Delete a Stack](workingstacks-shutting.md)