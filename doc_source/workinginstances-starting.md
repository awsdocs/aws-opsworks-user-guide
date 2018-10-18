# Manually Starting, Stopping, and Rebooting 24/7 Instances<a name="workinginstances-starting"></a>

**Note**  
You can use 24/7 instances with both Linux and Windows stacks\. 

After you add a 24/7 instance to a layer, you must manually start the instance to launch the corresponding Amazon Elastic Compute Cloud \(Amazon EC2 \) instance and manually stop it to terminate the Amazon EC2 instance\. You can also manually reboot instances that are not functioning properly\. AWS OpsWorks Stacks automatically starts and stops time\-based and load\-based instances\. For more information, see [Managing Load with Time\-based and Load\-based Instances](workinginstances-autoscaling.md)\.

**Important**  
AWS OpsWorks Stacks instances must be started, stopped, and restarted only in the AWS OpsWorks console\. AWS OpsWorks doesn't recognize start, stop, or restart operations performed in the Amazon EC2 console\.

**Topics**
+ [Starting or Restarting an Instance](#workinginstances-starting-start)
+ [Stopping an Instance](#workinginstances-starting-stop)
+ [Rebooting an Instance](#workinginstances-starting-reboot)

## Starting or Restarting an Instance<a name="workinginstances-starting-start"></a>

To start a new instance, on the **Instances** page, click **start** in the instance's **Actions** column\.

![\[start action on Instances page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/start_instance.png)

You can also create multiple instances and then start them all at the same time by clicking** Start all Instances**\.

After you start the instance, AWS OpsWorks Stacks launches an Amazon EC2 instance and boots the operating system\. The startup process usually takes a few minutes, and is typically somewhat slower for Windows instances than for Linux instances\. As startup progresses, the instance's **Status** field displays the following series of values: 

1. **requested** \- AWS OpsWorks Stacks has called the Amazon EC2 service to create the Amazon EC2 instance\.

1. **pending** \- AWS OpsWorks Stacks is waiting for the Amazon EC2 instance to start\.

1. **booting** \- The Amazon EC2 instance is booting\.

1. **running\_setup** \- AWS OpsWorks Stacks has triggered the Setup event and is running the layer's Setup recipes, followed by its Deploy recipes\. For more information, see [Executing Recipes](workingcookbook-executing.md)\. If you have [added custom cookbooks](workingcookbook-installingcustom-enable.md) to the stack, AWS OpsWorks Stacks installs the current version from your repository before running the Setup and Deploy recipes\.

1. **online** \- The instance is ready for use\.

When the **Status** changes to **online**, the instance is fully operational\.
+ If the layer has an attached load balancer, AWS OpsWorks Stacks adds the instance to it\.
+ AWS OpsWorks Stacks triggers a Configure event, which runs each instance's Configure recipes\.

  As needed, these recipes update the instance to accommodate the new instance\.
+ AWS OpsWorks Stacks replaces the instance's **start** action with **stop**, which you can use to stop the instance\. 

If the instance did not start successfully or the setup recipes failed, the status will be set to **start\_failed** or **setup\_failed**, respectively\. You can examine the logs to determine the cause\. For more information, see [Debugging and Troubleshooting Guide](troubleshoot.md)\.

A stopped instance remains part of the stack and retains all resources\. For example, Amazon EBS volumes and Elastic IP addresses are still associated with a stopped instance\. You can restart a stopped instance by choosing **start** in the instance's **Actions** column\. Restarting a stopped instance does the following:
+ Instance store\-backed instances – AWS OpsWorks Stacks launches a new Amazon EC2 instance with the same configuration\.
+ Amazon EBS\-backed instances – AWS OpsWorks Stacks restarts the Amazon EC2 instance, which reattaches the root volume\.

After the instance finishes booting, AWS OpsWorks Stacks installs operating system updates and runs the Setup and Deploy recipes, just as with the initial start\. AWS OpsWorks Stacks also does the following for restarted instances, as appropriate\.
+ Reassociates Elastic IP addresses\.
+ Reattaches Amazon Elastic Block Store \(Amazon EBS\) volumes\.
+ For instance store\-backed instances, installs the latest cookbook versions\.

  Amazon EBS\-backed instances continue to use the custom cookbooks that were stored on the root volume\. If your custom cookbooks have changed since you stopped the instance, you must manually update them after the instance is online\. For more information, see [Updating Custom Cookbooks](workingcookbook-installingcustom-enable-update.md)\. 

**Note**  
It might take several minutes for an Elastic IP address to be reassociated with a restarted instance\. Be aware that the instance's **Elastic IP** setting represents metadata, and simply indicates that the address should be associated with the instance\. The **Public IP** setting reflects the instance's state, and might be empty initially\. When the Elastic IP address is associated with the instance, the address is assigned to the **Public IP** setting, followed by \(EIP\)\.

## Stopping an Instance<a name="workinginstances-starting-stop"></a>

On the **Instances** page, click **stop** in the instance's **Actions** column, which notifies AWS OpsWorks Stacks to run the shutdown recipes and terminate the EC2 instance\. 

![\[stop action on Instances page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/stop_instance.png)

You can also shut down every instance in the stack by clicking **Stop All Instances**\. 

After you stop the instance, AWS OpsWorks Stacks performs several tasks:

1. If the instance's layer has an attached Elastic Load Balancing load balancer, AWS OpsWorks Stacks deregisters the instance\.

   If the layer supports the load balancer's connection draining feature, AWS OpsWorks Stacks delays triggering the Shutdown event until connection draining is complete\. For more information, see [Elastic Load Balancing Layer](layers-elb.md)\.

1. AWS OpsWorks Stacks triggers a Shutdown event, which runs the instance's Shutdown recipes\.

1. After triggering the Shutdown event, AWS OpsWorks Stacks waits for a specified time to allow the Shutdown recipes time to finish and then does the following:
   + Terminates instance store\-backed instances, which deletes all data\.
   + Stops Amazon EBS\-backed instances, which preserves the data on the root volume\.

   For more information on instance storage, see [Storage](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Storage.html)\.
**Note**  
The default shutdown timeout setting is 120 seconds\. If your Shutdown recipes need more time, you can [edit the layer configuration](workinglayers-basics-edit.md) to change the setting\.

You can monitor the shutdown process by watching the instance's **Status** column\. As shutdown progresses, it displays the following series of values: 

1. **terminating** \- AWS OpsWorks Stacks is terminating the Amazon EC2 instance\.

1. **shutting\_down** \- AWS OpsWorks Stacks is running the layer's Shutdown recipes\.

1. **terminated** \- The Amazon EC2 instance is terminated\.

1. **stopped** \- The instance has stopped\.

## Rebooting an Instance<a name="workinginstances-starting-reboot"></a>

On the **Instances** page, click the nonfunctioning instance's name to open the details page and then click **Reboot**\. 

![\[Reboot button on Instances page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/reboot_instance.png)

This command performs a soft reboot of the associated Amazon EC2 instance\. It does not delete the instance's data, even for instance store\-backed instances, and does not trigger any [lifecycle events](workingcookbook-events.md)\.

**Note**  
To have AWS OpsWorks Stacks automatically replace failed instances, enable auto healing\. For more information, see [Using Auto Healing](workinginstances-autohealing.md)\.