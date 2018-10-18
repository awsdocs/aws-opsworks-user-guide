# AWS OpsWorks Stacks Lifecycle Events<a name="workingcookbook-events"></a>

Each layer has a set of five lifecycle events, each of which has an associated set of recipes that are specific to the layer\. When an event occurs on a layer's instance, AWS OpsWorks Stacks automatically runs the appropriate set of recipes\. To provide a custom response to these events, implement custom recipes and [assign them to the appropriate events](workingcookbook-assigningcustom.md) for each layer\. AWS OpsWorks Stacks runs those recipes after the event's built\-in recipes\.

**Setup**  
This event occurs after a started instance has finished booting\. You can also manually trigger the Setup event by using the [Setup stack command](workingstacks-commands.md)\. AWS OpsWorks Stacks runs recipes that set the instance up according to its layer\. For example, if the instance is a member of the Rails App Server layer, the Setup recipes install Apache, Ruby Enterprise Edition, Passenger and Ruby on Rails\.  
A **Setup** event takes an instance out of service\. Because an instance is not in the **Online** state when the **Setup** lifecycle event runs, instances on which you run **Setup** events are removed from a load balancer\.

**Configure**  
This event occurs on all of the stack's instances when one of the following occurs:  
+ An instance enters or leaves the online state\.
+ You [associate an Elastic IP address](resources-attach.md#resources-attach-eip) with an instance or [disassociate one from an instance](resources-detach.md#resources-detach-eip)\.
+ You [attach an Elastic Load Balancing load balancer](layers-elb.md) to a layer, or detach one from a layer\.
For example, suppose that your stack has instances A, B, and C, and you start a new instance, D\. After D has finished running its setup recipes, AWS OpsWorks Stacks triggers the Configure event on A, B, C, and D\. If you subsequently stop A, AWS OpsWorks Stacks triggers the Configure event on B, C, and D\. AWS OpsWorks Stacks responds to the Configure event by running each layer's Configure recipes, which update the instances' configuration to reflect the current set of online instances\. The Configure event is therefore a good time to regenerate configuration files\. For example, the HAProxy Configure recipes reconfigure the load balancer to accommodate any changes in the set of online application server instances\.  
You can also manually trigger the Configure event by using the [Configure stack command](workingstacks-commands.md)\.

**Deploy**  
This event occurs when you run a **Deploy** command, typically to deploy an application to a set of application server instances\. The instances run recipes that deploy the application and any related files from its repository to the layer's instances\. For example, for a Rails Application Server instances, the Deploy recipes check out a specified Ruby application and tell [Phusion Passenger ](https://www.phusionpassenger.com/) to reload it\. You can also run Deploy on other instances so they can, for example, update their configuration to accommodate the newly deployed app\.  
Setup includes Deploy; it runs the Deploy recipes after setup is complete\.

**Undeploy**  
This event occurs when you delete an app or run an Undeploy command to remove an app from a set of application server instances\. The specified instances run recipes to remove all application versions and perform any required cleanup\.

**Shutdown**  
This event occurs after you direct AWS OpsWorks Stacks to shut an instance down but before the associated Amazon EC2 instance is actually terminated\. AWS OpsWorks Stacks runs recipes to perform cleanup tasks such as shutting down services\.  
 If you have attached an Elastic Load Balancing load balancer to the layer and [enabled support for connection draining](layers-elb.md), AWS OpsWorks Stacks waits until connection draining is complete before triggering the Shutdown event\.  
After triggering a Shutdown event, AWS OpsWorks Stacks allows Shutdown recipes a specified amount of time to perform their tasks, and then stops or terminates the Amazon EC2 instance\. The default Shutdown timeout value is 120 seconds\. If your Shutdown recipes might require more time, you can [edit the layer configuration](workinglayers-basics-edit.md#workinglayers-basics-edit-general) to change the timeout value\. For more information on instance Shutdown, see [Stopping an Instance](workinginstances-starting.md#workinginstances-starting-stop)\.

**Note**  
[Rebooting an instance](workinginstances-starting.md#workinginstances-starting-reboot) does not trigger any lifecycle events\.

For more discussion about the Deploy and Undeploy app commands, see [Deploying Apps](workingapps-deploying.md)\. 

After a started instance has finished booting, the remaining startup sequence is as follows:

1. AWS OpsWorks Stacks runs the instance's built\-in Setup recipes, followed by any custom Setup recipes\.

1. AWS OpsWorks Stacks runs the instance's built\-in Deploy recipes, followed by any custom Deploy recipes\.

   The instance is now online\.

1. AWS OpsWorks Stacks triggers a Configure event on all instances in the stack, including the newly started instance\.

   AWS OpsWorks Stacks runs the instances' built\-in Configure recipes, followed by any custom Configure recipes\.

**Note**  
To see the lifecycle events that have occurred on a particular instance, go to the **Instances** page and click the instance's name to open its details page\. The list of events is in the **Logs** section at the bottom of the page\. You can click **show** in the **Log** column to examine the Chef log for an event\. It provides detailed information about how the event was handled, including which recipes were run\. For more information on how to interpret Chef logs, see [Chef Logs](troubleshoot-debug-log.md)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/instance_logs.png)

For each lifecycle event, AWS OpsWorks Stacks installs a set of [stack configuration and deployment attributes](workingcookbook-json.md) on each instance that contains the current stack state and, for Deploy events, information about the deployment\. The attributes include information about what instances are available, their IP addresses, and so on\. For more information, see [Stack Configuration and Deployment Attributes](workingcookbook-json.md)\.

**Note**  
Starting or stopping a large number of instances at the same time can rapidly generate a large number of Configure events\. To avoid unnecessary processing, AWS OpsWorks Stacks responds to only the last event\. That event's stack configuration and deployment attributes contain all the information required to update the stack's instances for the entire set of changes\. This eliminates the need to also process the earlier Configure events\. AWS OpsWorks Stacks labels the unprocessed Configure events as **superseded**\.