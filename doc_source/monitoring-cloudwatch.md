# Monitoring Stacks using Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

AWS OpsWorks Stacks uses Amazon CloudWatch \(CloudWatch\) to provide monitoring for stacks\.
+ For Linux stacks, AWS OpsWorks Stacks supports thirteen custom metrics to provide detailed monitoring for each instance in the stack and summarizes the data for your convenience on the **Monitoring** page\.
+ For Windows stacks, you can monitor standard Amazon EC2 metrics for your instances with the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)\.

  The **Monitoring** page does not display Windows metrics\.

The **Monitoring** page displays metrics for an entire stack, a layer, or an instance\. AWS OpsWorks Stacks metrics are distinct from Amazon EC2 metrics\. You can also enable additional metrics through the CloudWatch console, but they typically require additional charges\. You can also view the underlying data on the CloudWatch console, as follows:

**To view OpsWorks custom metrics in CloudWatch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the navigation bar, select  the stack's region\.

1. In the navigation pane, choose **Metrics**\.

1. In OpsWorks Metrics, choose **Instance Metrics**, **Layer Metrics**, or **Stack Metrics**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/monitor_cloudwatch.png)

**Note**  
AWS OpsWorks Stacks collects metrics by running a process  on each instance \(the instance agent\)\. Because CloudWatch collects metrics differently, using the hypervisor, the values in the CloudWatch console might differ slightly from the corresponding values on the **Monitoring** page in the AWS OpsWorks Stacks console\.

You can also use CloudWatch console to set alarms\. For more information about how to create alarms, see [Creating Amazon CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\. For a list of CloudWatch custom metrics, see [AWS OpsWorks Metrics and Dimensions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/ops-metricscollected.html)\. For more information, see [Amazon CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)\. 

**Topics**
+ [AWS OpsWorks Stacks Metrics](#opsworks-metrics-dimensions)
+ [Dimensions for AWS OpsWorks Stacks Metrics](#opsworks-metricdimensions)
+ [Stack Metrics](#monitoring-cloudwatch-stack)
+ [Layer Metrics](#monitoring-cloudwatch-layer)
+ [Instance Metrics](#monitoring-cloudwatch-instance)

## AWS OpsWorks Stacks Metrics<a name="opsworks-metrics-dimensions"></a>

AWS OpsWorks Stacks sends the following metrics to CloudWatch every five minutes\.


**CPU Metrics**  

| Metric | Description | 
| --- | --- | 
|  `cpu_idle` |  The percentage of time that the CPU is idle\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_nice` |  The percentage of time that the CPU is handling processes with a positive `nice` value, which have a lower scheduling priority\. For more information about what this measures, see [nice \(Unix\)](http://en.wikipedia.org/wiki/Nice_(Unix))\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_steal` |  As AWS allocates hypervisor CPU resources among increasing numbers of instances, virtualization load rises, and can affect how often the hypervisor can perform requested work on an instance\. `cpu_steal` measures the percentage of time that an instance is waiting for the hypervisor to allocate physical CPU resources\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_system` |  The percentage of time that the CPU is handling system operations\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_user` |  The percentage of time that the CPU is handling user operations\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_waitio` |  The percentage of time that the CPU is waiting for input/output operations\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 


**Memory Metrics**  

| Metric | Description | 
| --- | --- | 
|  `memory_buffers` |  The amount of buffered memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_cached` |  The amount of cached memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_free` |  The amount of free memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_swap` |  The amount of swap space\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_total` |  The total amount of memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_used` |  The amount of memory in use\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 


**Load Metrics**  

| Metric | Description | 
| --- | --- | 
|  `load_1` |  The load averaged over a one\-minute window\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `load_5` |  The load averaged over a five\-minute window\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `load_15` |  The load averaged over a 15\-minute window\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 


**Process Metrics**  

| Metric | Description | 
| --- | --- | 
|  `procs` |  The number of active processes\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 

## Dimensions for AWS OpsWorks Stacks Metrics<a name="opsworks-metricdimensions"></a>

AWS OpsWorks Stacks metrics use the AWS OpsWorks Stacks namespace, and provide metrics for the following dimensions:


| Dimension | Description | 
| --- | --- | 
|  `StackId`  |  Average values for a stack\.  | 
|  `LayerId`  |  Average values for a layer\.  | 
|  `InstanceId`  |  Average values for an instance\.  | 

## Stack Metrics<a name="monitoring-cloudwatch-stack"></a>

To view a summary of metrics for an entire stack, select a stack in the AWS OpsWorks Stacks **Dashboard** and then click **Monitoring** in the navigation pane\. The following example is for a stack with a PHP and a DB layer\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/monitor_stack.png)

The stack view displays graphs of the four types of metrics for each layer over a specified time period: 1 hour, 8 hours, 24 hours, 1 week, or 2 weeks\. Note the following:
+ AWS OpsWorks Stacks periodically updates the graphs; the countdown timer at the upper right indicates the time remaining until the next update,
+ If a layer has more than one instance, the graphs display average values for the layer\.
+ You can specify the time period by clicking the list at the upper right and selecting your preferred value\. 

For each metric type, you can use the list at the top of the graph to select the particular metric that you want to view\.

## Layer Metrics<a name="monitoring-cloudwatch-layer"></a>

To view metrics for a particular layer, click the layer name in the **Monitoring Layers** view\. The following example shows metrics for the PHP layer, which has two instances\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/monitor_layer.png)

The metric types are the same as for the stack metrics, and for each type, you can use the list at the top of the graph to select the particular metric that you want to view\.

**Note**  
You can also display layer metrics by going to the layer's details page and clicking **Monitoring** at the upper right\.

## Instance Metrics<a name="monitoring-cloudwatch-instance"></a>

To view metrics for a particular instance, click the instance name in the layer monitoring view\. The following example shows metrics for the PHP layer's **php\-app1** instance\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/monitor_instance.png)

The graphs summarize all the available metrics for each metric type\. To get exact values for a particular point in time, use your mouse to move the slider \(indicated by the red arrow in the previous illustration\) to the appropriate position\.

**Note**  
You can also display instance metrics by going to the instance's details page and choosing **Monitoring** at the upper right\.