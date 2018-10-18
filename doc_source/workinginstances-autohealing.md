# Using Auto Healing to Replace Failed Instances<a name="workinginstances-autohealing"></a>

Every instance has an AWS OpsWorks Stacks agent that communicates regularly with the service\. AWS OpsWorks Stacks uses that communication to monitor instance health\. If an agent does not communicate with the service for more than approximately five minutes, AWS OpsWorks Stacks considers the instance to have failed\.

Auto healing is set at the layer level; you can change the auto healing setting by editing layer settings, as shown in the following screenshot\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/layer_auto_healing.png)

**Note**  
An instance can be a member of multiple layers\. If any of those layers has auto healing disabled, AWS OpsWorks Stacks does not heal the instance if it fails\.

If a layer has auto healing enabled—the default setting—AWS OpsWorks Stacks automatically replaces the layer's failed instances as follows:

**Instance store\-backed instance**  

1. Stops the Amazon EC2 instance, and verifies that it has shut down\.

1. Deletes the data on the root volume\.

1. Creates a new Amazon EC2 instance with the same host name, configuration, and layer membership\.

1. Reattaches any Amazon EBS volumes, including volumes that were attached after the old instance was originally started\.

1. Assigns a new public and private IP Address\.

1. If the old instance was associated with an Elastic IP address, associates the new instance with the same IP address\.

**Amazon EBS\-backed instance**  

1. Stops the Amazon EC2 instance, and verifies that it has stopped\.

1. Starts the EC2 instance\.

After the auto\-healed instance is back online, AWS OpsWorks Stacks triggers a Configure [lifecycle event](workingcookbook-events.md) on all of the stack's instances\. The associated [stack configuration and deployment attributes](workingcookbook-json.md) include the instance's public and private IP addresses\. Custom Configure recipes can obtain the new IP addresses from the node object\.

If you [specify an Amazon EBS volume](workinglayers-basics-edit.md#workinglayers-basics-edit-ebs) for a layer's instances, AWS OpsWorks Stacks creates a new volume and attaches it to each instance when the instance is started\. If you later want to detach the volume from an instance, use the [Resources](resources.md) page\. 

When AWS OpsWorks Stacks auto heals one of a layer's instances, it handles volumes in the following way:
+ If the volume was attached to the instance when the instance failed, the volume and its data are saved, and AWS OpsWorks Stacks attaches it to the new instance\.
+ If the volume was not attached to the instance when the instance failed, AWS OpsWorks Stacks creates a new, empty volume with the configuration specified by the layer, and attaches that volume to the new instance\.

Auto healing is enabled by default for all layers, but you can [edit the layer's General Settings](workinglayers-basics-edit.md) to disable it\.

**Important**  
If you have auto healing enabled, be sure to do the following:   
Use only the AWS OpsWorks Stacks console, CLI, or API to stop instances\.  
If you stop an instance in any other way, such as using the Amazon EC2 console, AWS OpsWorks Stacks treats the instance as failed, and auto heals it\. 
Use Amazon EBS volumes to store any data that you don't want to lose if the instance is auto healed\.  
Auto healing stops the old Amazon EC2 instance, which destroys any data that is not stored on an Amazon EBS volume\. Amazon EBS volumes are reattached to the new instance, which preserves any stored data\.