# Using Automatic Time\-based Scaling<a name="workinginstances-autoscaling-timebased"></a>

Time\-based scaling lets you control how many instances a layer should have online at certain times of day or days of the week by starting or stopping instances on a specified schedule\. AWS OpsWorks Stacks checks every couple of minutes and starts or stops instances as required\. You specify the schedule separately for each instance, as follows:
+ Time of day\. You can have more instances running during the day than at night, for example\. 
+ Day of the week\. You can have more instances running on weekdays than weekends, for example\. 

**Note**  
You cannot specify particular dates\.

**Topics**
+ [Adding a Time\-Based Instance to a Layer](#workinginstances-autoscaling-timebased-add)
+ [Configuring a Time\-Based Instance](#workinginstances-autoscaling-timebased-configure)

## Adding a Time\-Based Instance to a Layer<a name="workinginstances-autoscaling-timebased-add"></a>

You can either add a new time\-based instance to a layer, or use an existing instance\.

**To add a new time\-based instance**

1. On the **Instances** page, click **\+ Instance** to add an instance\. On the **New** tab, click **Advanced >>** and then click **time\-based**\.  
![\[Time-based scaling option on Add instance page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/time_based_instances.png)

1. Configure the instance as desired\. Then click **Add Instance** to add the instance to the layer\.

**To add an existing time\-based instance to a layer**

1. On the **Time\-based Instances** page, click **\+ Instance** if a layer already has a time\-based instance\. Otherwise, click **Add a time\-based instance**\. Then click the **Existing** tab\.  
![\[Add existing time-based instance to a layer\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/time_based_instances_existing.png)

1. On the **Existing** tab, select an instance from the list\. The list shows only time\-based instances\.
**Note**  
If you change your mind about using an existing instance, click the **New** tab to create a new instance, as described in the preceding procedure\.

1. Click **Add instance** to add the instance to the layer\.

## Configuring a Time\-Based Instance<a name="workinginstances-autoscaling-timebased-configure"></a>

After you add a time\-based instance to a layer, you configure its schedule as follows\.

**To configure a time\-based instance**

1. In the navigation pane, click **Time\-based** under **Instances**\.

1. Specify the online periods for each time\-based instance by clicking the appropriate boxes below the desired hour\.
   + To use the same schedule every day, click the **Every day** tab and then specify the online time periods\.
   + To use different schedules on different days, click each day and select the appropriate time periods\.   
![\[Schedule for time-based scaling\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/time_based.png)

**Note**  
Make sure to allow for the amount of time it takes to start an instance and the fact that AWS OpsWorks Stacks checks only every few minutes to see if instances should be started or stopped\. For example, if an instance should be running by 1:00 UTC, start it at 0:00 UTC\. Otherwise, AWS OpsWorks Stacks might not start the instance until several minutes past 1:00 UTC, and it will take several more minutes for it to come online\.

You can modify an instance's online time periods at any time using the previous configuration steps\. The next time AWS OpsWorks Stacks checks, it uses the new schedule to determine whether to start or stop instances\.

**Note**  
You can also add a new time\-based instance to a layer by going to the **Time\-based** page and clicking **Add a time\-based instance** \(if you have not yet added a time\-based instance to the layer\) or **\+Instance** \(if the layer already has one or more time\-based instances\)\. Then configure the instance as described in the preceding procedures\.