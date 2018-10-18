# Using Automatic Load\-based Scaling<a name="workinginstances-autoscaling-loadbased"></a>

Load\-based instances let you rapidly start or stop instances in response to changes in incoming traffic\. AWS OpsWorks Stacks uses [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/) data to compute the following metrics for each layer, which represent average values across all of the layer's instances:
+ CPU: The average CPU consumption, such as 80%
+ Memory: The average memory consumption, such as 60%
+ Load: The average computational work a system performs in one minute\. 

You define *upscaling* and *downscaling* thresholds for any or all of these metrics\. You can also use custom CloudWatch alarms as thresholds\.

Crossing a threshold triggers a *scaling event*\. You determine how AWS OpsWorks Stacks responds to scaling events by specifying the following:
+ How many instances to start or stop\.
+ How long AWS OpsWorks Stacks should wait after exceeding a threshold before starting or deleting instances\. For example, CPU utilization must exceed the threshold for at least 15 minutes\. This value allows you to ignore brief traffic fluctuations\.
+ How long AWS OpsWorks Stacks should wait after starting or stopping instances before monitoring metrics again\. You usually want to allow enough time for started instances to come online or stopped instances to shut down before assessing whether the layer is still exceeding a threshold\. 

When a scaling event occurs, AWS OpsWorks Stacks starts or stops only load\-based instances\. It does not start or stop 24/7 instances or time\-based instances\. 

**Note**  
Automatic load\-based scaling does not create new instances; it starts and stops only those instances that you have created\. You must therefore provision enough load\-based instances in advance to handle the maximum anticipated load\.

**To create a load\-based instance**

1. On the **Instances** page, click **\+Instance** to add an instance\. Click **Advanced >>** and then click **load\-based**\.  
![\[Load-based scaling option on Add instance page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/load_based_instances.png)

1. Configure the instance as desired\. Then click **Add Instance **to add the instance to the layer\.

Repeat this procedure until you have created a sufficient number of instances\. You can add or remove instances later, as required\.

After you have added load\-based instances to a layer, you must enable load\-based scaling and specify the configuration\. The load\-based scaling configuration is a layer property, not an instance property, that specifies when a layer should start or stop its load\-based instances\. It must be specified separately for each layer that uses load\-based instances\. 

**To enable and configure automatic load\-based scaling**

1. In the navigation pane, click **Load\-based** under **Instances** and click **edit** for the appropriate layer\.  
![\[edit action on instance layer\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/load_based.png)

1. Set **Load\-based auto scaling enabled** to **Yes**\. Then set threshold and scaling parameters to define how and when to add or delete instances\.  
![\[Thresholds for load-based scaling\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/load_based_config.png)  
**Layer\-average thresholds**  
You can set scaling thresholds based on the following values, which are averaged over all of the layer's instances\.  
   + **Average CPU** – The layer's average CPU utilization, as a percent of the total\.
   + **Average memory** – The layer's average memory utilization, as a percent of the total\.
   + **Average load** – The layer's average load\.

     For more information about how load is computed, see [Load \(computing\)](http://en.wikipedia.org/wiki/Load_(computing))\.
Crossing a threshold causes a scaling event, upscaling if more instances are needed and downscaling if fewer instances are needed\. AWS OpsWorks Stacks then adds or deletes instances based on the scaling parameters\.  
**Custom CloudWatch alarms**  
You can use up to five custom CloudWatch alarms as upscaling or downscaling thresholds\. They must be in the same region as the stack\. For more information about how to create custom alarms, see [Creating Amazon CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/AlarmThatSendsEmail.html)\.  
To use custom alarms, you must update your service role to allow `cloudwatch:DescribeAlarms`\. You can either have AWS OpsWorks Stacks update the role for you when you first use this feature or you can edit the role manually\. For more information, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\.  
**Scaling parameters**  
These parameters control how AWS OpsWorks Stacks manages scaling events\.  
   + **Start servers in batches of** – The number of instances to add or remove when the scaling event occurs\.
   + **If thresholds are exceeded** – The amount of time \(in minutes\), that the load must remain over an upscaling threshold or under a downscaling threshold before AWS OpsWorks Stacks triggers a scaling event\.
   + **After scaling, ignore metrics ** – The amount of time \(in minutes\) after a scaling event occurs that AWS OpsWorks Stacks should ignore metrics and suppress additional scaling events\.

     For example, AWS OpsWorks Stacks adds new instances following an upscaling event but the instances won't start reducing the load until they have been booted and configured\. There is no point in raising additional scaling events until the new instances are online and handling requests, which typically takes several minutes\. This setting allows you to direct AWS OpsWorks Stacks to suppress scaling events long enough to get the new instances online\.

     You can also increase this setting to prevent sudden swings in scaling when layer averages such as **Average CPU**, **Average memory**, or **Average load** are in temporary disagreement\.

     For example, if CPU usage is above the limit and memory usage is close to being downscaled, an instance upscale event could be immediately followed by a memory downscaling event\. To prevent this from happening, you can increase the number of minutes in the **After scaling, ignore metrics** setting\. In the example mentioned, the CPU scaling would occur, but the memory downscaling event would not\.

1. To add additional load\-based instances, click **\+ Instance**, configure the settings, and click **Add Instance**\. Repeat until you have enough load\-based instances to handle your maximum anticipated load\. Then click **Save**\.

**Note**  
You can also add a new load\-based instance to a layer by going to the **Load\-based** page and clicking **Add a load\-based instance** \(if you have not yet added a load\-based instance to the layer\) or **\+Instance** \(if the layer already has one or more load\-based instances\. Then configure the instance as described earlier in this section\.

**To add an existing load\-based instance to a layer**

1. In the navigation pane, click **Load\-based** under **Instances**\.

1. If you have already enabled load\-based automatic scaling for a layer, click **\+Instance**\. Otherwise, click **Add a load\-based instance**\. Then click the **Existing** tab\.  
![\[Add existing load-based instance to a layer\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/load_based_instances_existing.png)

1. On the **Existing** tab, select an instance from the list\. The list shows only load\-based instances\.
**Note**  
If you change your mind about using an existing instance, click the **New** tab to create a new instance as described in the preceding procedure\.

1. Click **Add Instance** to add the instance to the layer\.

You can modify the configuration for or disable automatic load\-based scaling at any time\.

**To disable automatic load\-based scaling**

1. In the navigation pane, click **Load\-based** under **Instances** and click **edit** for the appropriate layer\.

1. Toggle **Load\-based auto scaling enabled** to **No**\.