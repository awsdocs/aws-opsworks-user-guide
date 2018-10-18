# Editing the Instance Configuration<a name="workinginstances-properties"></a>

You can edit instance configurations, including [registered Amazon Elastic Compute Cloud \(Amazon EC2\) instances](registered-instances.md), with the following limitations:
+ The instance must be in the stopped state\.

  Although you can't modify an online instance's properties, you can change some aspects of its configuration by editing the instance's layers\. For more information, see [Editing an OpsWorks Layer's Configuration](workinglayers-basics-edit.md)\.
+ Some settings, such as **Availability Zone** and **Scaling Type**, are determined when you create the instance and cannot be modified later\.
+ Some settings can be modified for instance store\-backed instances only, not for Amazon Elastic Block Store\-backed instances\.

  For example, you can change an instance store\-backed instance's operating system\. Amazon EBS\-backed instances must use the operating system that you specified when you created the instance\. For more information on instance storage, see [Storage](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Storage.html)\.
+ By default, instances inherit the [stack's agent version](workingstacks-creating.md#workingstacks-creating-advanced-agent) setting\.

  You can use **OpsWorks Agent Version** to override the stack's agent version setting and specify a particular agent version for an instance\. If you specify an instance's agent version, AWS OpsWorks Stacks does not automatically update the agent when a new version is available, even if the stack's agent version setting is **Auto\-update**\. You must update the instance's agent version manually by editing the instance configuration\. AWS OpsWorks Stacks then installs the specified agent version on the instance\. 

**Note**  
You cannot edit the configuration of registered on\-premises instances\.

**To edit an instance's configuration**

1. Stop the instance, if it is not already stopped\.

1. On the **Instances** page, click an instance name to display the **Details** page\.

1. Click **Edit** to display the edit page\.

1. Edit the instance's configuration, as appropriate\.

For a description of the **Host name**, **Size**, **SSH key** and **Operating system** settings, see [Adding an Instance to a Layer](workinginstances-add.md)\. The **Layers** setting lets you add or remove layers\. The instance's current layer's appear following the list of layers\.
+ To add another layer, select it from the list\.
+ To remove the instance from one of its layers, click the **x** by the appropriate layer\.

  An instance must be a member of at least one layer, so you cannot remove the last layer\. 

When you restart the instance, AWS OpsWorks Stacks starts a new Amazon EC2 instance with the updated configuration\.