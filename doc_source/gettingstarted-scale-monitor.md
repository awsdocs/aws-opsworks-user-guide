# Step 4\.3: Monitor MyStack<a name="gettingstarted-scale-monitor"></a>

AWS OpsWorks Stacks uses Amazon CloudWatch to provide metrics for a stack and summarizes them for your convenience on the **Monitoring** page\. You can view metrics for the entire stack, a specified layer, or a specified instance\. 

**To monitor MyStack**

1. In the navigation pane, click **Monitoring**, which displays a set of graphs with average metrics for each layer\. You can use the menus for **CPU System**, **Memory Used**, and **Load** to display different related metrics\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/monitor_stack.png)

1. Click **PHP App Server** to see metrics for each of the layer's instances\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/monitor_layer.png)

1. Click **php\-app1** to see metrics for that instance\. You can see metrics for any particular point in time by moving the slider\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/monitor_instance.png)

**Note**  
AWS OpsWorks Stacks also supports the Ganglia monitoring server, which might have advantages for some applications\. For more information, see [Ganglia Layer](workinglayers-ganglia.md)\.