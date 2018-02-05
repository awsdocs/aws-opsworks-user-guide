# Shut Down a Stack<a name="workingstacks-shutting"></a>

If you no longer need a stack, you can shut it down\.

**To shut down a stack**

1. On the AWS OpsWorks Stacks dashboard, click the stack that you want to shut down\.

1. In the navigation pane, click **Instances**\.

1. On the **Instances** page, click **Stop all Instances**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/stop_all_instances.png)

1. After the instances have stopped, for each instance in the layer, click **delete** in the **Actions** column\. When prompted to confirm, click **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_instance.png)

1. When all the instances are deleted, in the navigation pane, click **Layers**\.

1. On the **Layers** page, for each layer in the stack, click **delete**\. When a confirmation prompt appears, click **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_layer.png)

1. When all the layers are deleted, in the navigation pane, click **Apps**\.

1. On the **Apps** page, for each app in the stack, click ** delete** in the **Actions** column\. When prompted to confirm, click **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_app.png)

1. When all the apps are deleted, in the navigation pane, click **Stack**\.

1. On the stack page, click **Delete stack**\. When prompted to confirm, click **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_stack.png)