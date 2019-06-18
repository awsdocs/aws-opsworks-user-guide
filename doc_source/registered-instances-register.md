# Registering an Instance with an AWS OpsWorks Stacks Stack<a name="registered-instances-register"></a>

**Note**  
This feature is supported only for Linux stacks\.

To register an instance that is outside of AWS OpsWorks Stacks, you run the `register` AWS CLI command\. You can run this command from the instance that you want to register, or from another computer\. You apply the `AWSOpsWorksRegisterCLI_EC2` or `AWSOpsWorksRegisterCLI_OnPremises` policies to an IAM user or group to grant permissions required for the AWS CLI to register EC2 or on\-premises instances, respectively\. These policies require version 1\.16\.180 of the AWS CLI or newer\.

The registration process installs an agent on an instance that you want to manage by using AWS OpsWorks Stacks, and registers the instance with an AWS OpsWorks stack that you specify\. After you register an instance, the instance is part of the stack and is managed by AWS OpsWorks Stacks\. For more information, see [Managing Registered Instances](registered-instances-manage.md)\.

**Note**  
Although [AWS Tools for PowerShell](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-welcome.html) includes the [http://docs.aws.amazon.com/powershell/latest/reference/items/Register-OPSInstance.html](http://docs.aws.amazon.com/powershell/latest/reference/items/Register-OPSInstance.html) cmdlet, which calls the `register` API action, we recommend that you use the AWS CLI to run the `register` command instead\.

The following diagram shows both approaches to registering an Amazon EC2 instance\. You can use the same approaches for registering an on\-premises instance\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/on-prem-provision.png)

**Note**  
You can use the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) to manage a registered instance, but you must run an AWS CLI `register` command to register the instance\. The reason for this is that the registration process must be run from the instance, which can't be done by the console\.

The following sections describe the procedure in detail\.

**Topics**
+ [Walkthrough: Register an Instance from Your Workstation](registered-instances-register-walkthrough.md)
+ [Registering Amazon EC2 and On\-premises Instances](registered-instances-register-registering.md)