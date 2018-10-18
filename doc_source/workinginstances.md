# Instances<a name="workinginstances"></a>

An instance represents a computing resource, such as an Amazon EC2 instance, which handles the work of serving applications, balancing traffic, and so on\. An instance's operating system can have any of several Linux distributions, or Windows Server 2012 R2\.

You can add instances to a stack in either of the following ways:
+ Use AWS OpsWorks Stacks to add instances to a stack\. The instances that you add represent Amazon EC2 instances\.
+ For Linux\-based stacks, you can register instances that were created elsewhere—including instances that you created with Amazon EC2 and *on\-premises* instances that are running on your own hardware\.

  You can then use AWS OpsWorks Stacks to manage these instances in much the same way as instances created with AWS OpsWorks Stacks

 This section describes how to use AWS OpsWorks Stacks to create and manage instances\.

**Topics**
+ [Using AWS OpsWorks Stacks Instances](workinginstances-opsworks.md)
+ [Using Computing Resources Created Outside of AWS OpsWorks Stacks](registered-instances.md)
+ [Editing the Instance Configuration](workinginstances-properties.md)
+ [Deleting AWS OpsWorks Stacks Instances](workinginstances-delete.md)
+ [Using SSH to Log In to a Linux Instance](workinginstances-ssh.md)
+ [Using RDP to Log In to a Windows Instance](workinginstances-rdp.md)