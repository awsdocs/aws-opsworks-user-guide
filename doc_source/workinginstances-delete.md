# Deleting AWS OpsWorks Stacks Instances<a name="workinginstances-delete"></a>

**Important**  
This topic applies only to Amazon EC2 instances that are managed by AWS OpsWorks Stacks\. For more information about how to delete instances that are managed by the Amazon EC2 console or API, see [Terminate Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)\.

You can use AWS OpsWorks Stacks to stop an instance, including [registered Amazon EC2 instances](registered-instances.md)\. Doing so stops the EC2 instance, but the instance remains in the stack\. You can restart it by clicking **start** in the instance's **Actions** column\. If you no longer need an instance and want to remove it from the stack, you can delete it, which removes the instance from the stack and terminates the associated Amazon EC2 instance\. Deleting an instance also deletes any associated logs or data, and any Amazon Elastic Block Store \(EBS\) volumes on the instance\.

**Note**  
You cannot use AWS OpsWorks Stacks to delete a registered on\-premises instance\.

If an instance belongs to multiple layers, you can delete the instance from the stack or just remove a particular layer\. You can also remove layers from instances by editing the instance configuration, as described in [Editing the Instance Configuration](workinginstances-properties.md)\.

**Important**  
You should delete AWS OpsWorks Stacks instances only by using the AWS OpsWorks Stacks console or API\. In particular, you should not delete AWS OpsWorks Stacks instances by using the Amazon EC2 console or API because Amazon EC2 actions are not automatically synchronized with AWS OpsWorks Stacks\. For example, if auto healing is enabled and you terminate an instance by using the Amazon EC2 console, AWS OpsWorks Stacks treats the terminated instance as a failed instance and launches another Amazon EC2 instance to replace it\. For more information, see [Using Auto Healing](workinginstances-autohealing.md)\. 

**To delete an instance**

1. On the **Instances** page, locate the instance under the appropriate layer\. If the instance is running, click **stop** in the **Actions** column\.

1. After the status changes to **stopped**, click **delete**\. If the instance is a member of more than one layer, layer AWS OpsWorks Stacks displays the following section\.  
![\[delete action on Instances page for an instance that belongs to multiple layers\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_instance_multiple.png)
   + To remove the instance from only the selected layer, click **Remove from layer**\.

     The instance remains a member of its other layers and can be restarted\.
   + To delete the instance from all its layers, which removes it from the stack, click **here**\.

1. If you choose to completely remove an instance from the stack, or if the instance is a member of only one layer, AWS OpsWorks Stacks displays the following section\.  
![\[delete action on Instances page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_instance.png)

   Click **Yes, delete** to confirm\. In addition to deleting the instance from the stack, this action deletes any associated logs or data; it also deletes any Amazon EBS volumes on the instance\.