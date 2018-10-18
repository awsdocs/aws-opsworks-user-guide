# Registering an Instance with an AWS OpsWorks Stacks Stack<a name="registered-instances-register"></a>

**Note**  
This feature is supported only for Linux stacks\.

The registration process installs an agent on the instance, which AWS OpsWorks Stacks then uses to manage the instance, and registers the instance with the specified stack\. At that point, the instance is part of the stack and is managed by AWS OpsWorks Stacks\. For more information, see [Managing Registered Instances](registered-instances-manage.md)\.

You register an instance by running the `register` AWS CLI command\. You can run this command from the instance itself, or from a separate workstation\. 

**Note**  
The `register` command is not included in the [AWS Tools for Windows PowerShell](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-welcome.html)\.

The following diagram shows both approaches to registering an Amazon EC2 instance\. You can use the same approaches for registering an on\-premises instance\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/on-prem-provision.png)

**Note**  
You can use the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) to manage a registered instance, but you must use an AWS CLI command to register the instance\. The reason for this requirement is that the registration process must be run from the instance, which can't be done by the console\.

The following sections describe the procedure in detail\.

**Topics**
+ [Walkthrough: Register an Instance from Your Workstation](registered-instances-register-walkthrough.md)
+ [Registering Amazon EC2 and On\-premises Instances](registered-instances-register-registering.md)