# Managing Load with Time\-based and Load\-based Instances<a name="workinginstances-autoscaling"></a>

As your incoming traffic varies, your stack may have either too few instances to comfortably handle the load or more instances than necessary\. You can save both time and money by using time\-based or load\-based instances to automatically increase or decrease a layer's instances so that you always have enough instances to adequately handle incoming traffic without paying for unneeded capacity\. There's no need to monitor server loads or manually start or stop instances\. In addition, time\- and load\-based instances automatically distribute, scale, and balance applications over multiple Availability Zones within a region, giving you geographic redundancy and scalability\.

Automatic scaling is based on two instance types, which adjust a layer's online instances based on different criteria: 
+ **Time\-based** instances

  They allow a stack to handle loads that follow a predictable pattern by including instances that run only at certain times or on certain days\. For example, you could start some instances after 6PM to perform nightly backup tasks or stop some instances on weekends when traffic is lower\. 
+ **Load\-based** instances

  They allow a stack to handle variable loads by starting additional instances when traffic is high and stopping instances when traffic is low, based on any of several load metrics\. For example, you can have AWS OpsWorks Stacks start instances when the average CPU utilization exceeds 80% and stop instances when the average CPU load falls below 60%\.

Both time\-based and load\-based instances are supported for Linux stacks, while only time\-based instances are supported for Windows stacks\.

Unlike 24/7 instances, which you must start and stop manually, you do not start or stop time\-based or load\-based instances yourself\. Instead, you configure the instances and AWS OpsWorks Stacks starts or stops them based on their configuration\. For example, you configure time\-based instances to start and stop on a specified schedule\. AWS OpsWorks Stacks then starts and stops the instances according to that configuration\.

A common practice is to use all three instance types together, as follows\.
+ A set 24/7 instances to handle the base load\. You typically just start these instances and let them run continuously\.
+ A set of time\-based instances, which AWS OpsWorks Stacks starts and stops to handle predictable traffic variations\. For example, if your traffic is highest during working hours, you would configure the time\-based instances to start in the morning and shut down in the evening\.
+ A set of load\-based instances, which AWS OpsWorks Stacks starts and stops to handle unpredictable traffic variations\. AWS OpsWorks Stacks starts them when the load approaches the capacity of the stacks' 24/7 and time\-based instances, and stops them when the traffic returns to normal\.\.

For more information on how to use these scaling times, see [Optimizing the Number of Servers](best-practices-autoscale.md)\.

**Note**  
If you have created apps for the instances' layer or created custom cookbooks, AWS OpsWorks Stacks automatically deploys the latest version to time\-based and load\-based instances when they are first started\. However, AWS OpsWorks Stacks does not necessarily deploy the latest cookbooks to restarted offline instances\. For more information, see [Editing Apps](workingapps-editing.md) and [Updating Custom Cookbooks](workingcookbook-installingcustom-enable-update.md)\. 

**Topics**
+ [Using Automatic Time\-based Scaling](workinginstances-autoscaling-timebased.md)
+ [Using Automatic Load\-based Scaling](workinginstances-autoscaling-loadbased.md)
+ [How Load\-based Scaling Differs from Auto Healing](#workinginstances-autoscaling-differs)

## How Load\-based Scaling Differs from Auto Healing<a name="workinginstances-autoscaling-differs"></a>

Automatic load\-based scaling uses load metrics that are averaged across all running instances\. If the metrics remain between the specified thresholds, AWS OpsWorks Stacks does not start or stop any instances\. With auto healing, on the other hand, AWS OpsWorks Stacks automatically starts a new instance with the same configuration when an instance stops responding\. The instance may not be able to respond due to a network issue or some problem with the instance\.

For example, suppose that your CPU upscaling threshold is 80%, and then one instance stops responding\. 
+ If auto healing is disabled, and the remaining running instances are able to keep average CPU utilization below 80%, AWS OpsWorks Stacks does not start a new instance\. It starts a replacement instance only if the average CPU utilization across the remaining instances exceeds 80%\.
+ If auto healing is enabled, AWS OpsWorks Stacks starts a replacement instance irrespective of the load thresholds\. 