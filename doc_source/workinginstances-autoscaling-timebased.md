# Using automatic time\-based scaling<a name="workinginstances-autoscaling-timebased"></a>

Time\-based scaling lets you control how many instances a layer should have online at certain times of day or days of the week by starting or stopping instances on a specified schedule\. AWS OpsWorks Stacks checks every couple of minutes and starts or stops instances as required\. You specify the schedule separately for each instance, as follows:
+ Time of day\. You can have more instances running during the day than at night, for example\. 
+ Day of the week\. You can have more instances running on weekdays than weekends, for example\. 

**Note**  
You cannot specify particular dates\.

**Topics**
+ [Adding a time\-based instance to a layer](#workinginstances-autoscaling-timebased-add)
+ [Configuring a time\-based instance](#workinginstances-autoscaling-timebased-configure)

## Adding a time\-based instance to a layer<a name="workinginstances-autoscaling-timebased-add"></a>

You can either add a new time\-based instance to a layer, or use an existing instance\.

**To add a new time\-based instance**

1. On the **Instances** page, choose **\+ Instance** to add an instance\. On the **New** tab, choose **Advanced**, and then choose **time\-based**\.  
![\[Time-based scaling option on Add instance page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/time_based_instances.png)

1. Configure the instance\. Then choose **Add Instance** to add the instance to the layer\.

**To add an existing time\-based instance to a layer**

1. On the **Time\-based Instances** page, choose **\+ Instance** if a layer already has a time\-based instance\. Otherwise, choose **Add a time\-based instance**\. Then choose the **Existing** tab\.  
![\[Add existing time-based instance to a layer\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/time_based_instances_existing.png)

1. On the **Existing** tab, choose an instance from the list\. The list shows only time\-based instances\.
**Note**  
If you change your mind about using an existing instance, on the **New** tab, create a new instance, as described in the preceding procedure\.

1. Choose **Add instance** to add the instance to the layer\.

## Configuring a time\-based instance<a name="workinginstances-autoscaling-timebased-configure"></a>

After you add a time\-based instance to a layer, you configure its schedule as follows\.

**To configure a time\-based instance**

1. In the navigation pane, under **Instances**, choose **Time\-based**\.

1. Specify the online periods for each time\-based instance by filling the appropriate boxes below the desired hour\.
   + To use the same schedule every day, choose the **Every day** tab, and then specify the online time periods\.
   + To use different schedules on different days, choose each day, and then choose the appropriate time periods\.  
![\[Schedule for time-based scaling\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/time_based.png)

**Note**  
Be sure to allow for the amount of time it takes to start an instance, and that AWS OpsWorks Stacks checks only every few minutes to see if instances should be started or stopped\. For example, if an instance should be running by 1:00 UTC, start it at 0:00 UTC\. Otherwise, AWS OpsWorks Stacks might not start the instance until several minutes past 1:00 UTC, and the instance takes several more minutes to be online\.

You can change an instance's online time periods at any time by performing the preceding steps\. The next time AWS OpsWorks Stacks checks, it uses the new schedule to determine whether to start or stop instances\.

**Note**  
You can add a new time\-based instance to a layer by opening the **Time\-based** page, and choosing **Add a time\-based instance** \(if you have not yet added a time\-based instance to the layer\) or **\+ Instance** \(if the layer already has one or more time\-based instances\)\. Then, configure the instance as described in the preceding procedures\.