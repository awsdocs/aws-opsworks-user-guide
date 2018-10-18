# Assigning a Registered Instance to a Layer<a name="registered-instances-assign"></a>

**Note**  
This feature is supported only for Linux stacks\.

After you register an instance, you can assign it to one or more layers\. The advantage of assigning an instance to a layer instead of leaving it unassigned is that you can assign custom recipes to the layer's [lifecycle events](workingcookbook-events.md)\. AWS OpsWorks Stacks then runs them automatically at the appropriate time, after the layer's recipes for that event\.
+ You can assign any registered instance to a [custom layer](workinglayers-custom.md)\. A custom layer has a minimal set of recipes that do not install any packages, so they should not create any conflicts with the instance's existing configuration\. 
+ You can assign on\-premises instances to AWS OpsWorks Stacks [built\-in layers](workinglayers.md)\.

  Every built\-in layer includes recipes that automatically install one or more packages\. For example, the Java App Server Setup recipes install Apache and Tomcat\. The layer's recipes might also perform other operations such as restarting services and deploying applications\. Before assigning an on\-premises instance to a built\-in layer, you should ensure that the layer's recipes do not create any conflicts, such as attempting to install a different application server version than is currently on the instance\. For more information, see [Layers](workinglayers.md) and [AWS OpsWorks Stacks Layer Reference](layers.md)\.

**To assign a registered instance to a layer**

1. Add the layers that you want to use to the stack, if you have not done so already\.

1. Choose **Instances** in the navigation pane and then choose **assign** in the instance's **Actions** column\.

1. Select the appropriate layers and choose **Save**\.

When you assign an instance to a layer AWS OpsWorks Stacks does the following\.
+ Runs the layer's Setup recipes\.
+ Adds any attached Elastic IP addresses or Amazon EBS volumes to the stack's resources\.

  You can then use AWS OpsWorks Stacks to manage these resources\. For more information, see [Resource Management](resources.md)\.

After they are finished, the instance is in the online status and fully incorporated into the stack\. AWS OpsWorks Stacks then runs the layer's assigned recipes each time a lifecycle event occurs\.