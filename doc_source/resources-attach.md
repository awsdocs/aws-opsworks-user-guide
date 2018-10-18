# Attaching and Moving Resources<a name="resources-attach"></a>

After you register a resource with a stack, you can attach it to one of the stack's instances\. You can also move an attached resource from one instance to another\. Note the following:
+ When you attach or move Amazon EBS volumes, the instances involved in the operation must be offline\. If the instance you are interested in is not on the **Resources** page, go to the **Instances** page and [stop the instance](workinginstances-starting.md)\. After it has stopped, you can return to the **Resources** page and attach or move the resource\.
+ When you attach or move Elastic IP addresses, the instances can be online or offline\.
+ If you delete an instance, any attached resources remain registered with the stack\. You can then attach the resource to another instance or, if you no longer need it, deregister the resource\.

**Topics**
+ [Assigning Amazon EBS Volumes to an Instance](#resources-attach-ebs)
+ [Associating Elastic IP Addresses with an Instance](#resources-attach-eip)
+ [Attaching Amazon RDS Instances to an App](#resources-attach-rds)

## Assigning Amazon EBS Volumes to an Instance<a name="resources-attach-ebs"></a>

**Note**  
You cannot assign Amazon EBS volumes to Windows instances\. 

You can assign a registered Amazon EBS volume to an instance and move it from one instance to another, but both instances must be offline\.

**To assign an Amazon EBS volume to an instance**

1. On the Resources page, click **assign to instance** in the appropriate volume's **Instance** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-ebs4.png)

1. On the volume's details page, select the appropriate instance, specify the volume's name and mount point, and click **Save** to attach the volume to the instance\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-ebs5.png)

**Important**  
If you have assigned an external in\-use volume to your instance, you must use the Amazon EC2 console, API, or CLI to unassign it from the original instance or the start process will fail\. 

You can also use the details page to move an assigned Amazon EBS volume to another instance in the stack\.

**To move an Amazon EBS volume to another instance**

1. Ensure that both instances are in the offline state\.

1. On the **Resources** page, click **Volumes** and then click **edit** in the volume's **Actions** column\.

1. Do one of the following:
   + To move the volume to another instance in the stack, select the appropriate instance from the **Instance** list and click **Save**\.
   + To move the volume to an instance in another stack, [deregister the volume](resources-dereg.md), [register the volume](resources-reg.md) with the new stack, and [attach it](#resources-attach) to the news instance\.

## Associating Elastic IP Addresses with an Instance<a name="resources-attach-eip"></a>

You can associate a registered Elastic IP address with an instance and move it from one instance to another, including instances in other stacks\. The instances can be either online or offline\.

**To associate an Elastic IP address with an instance**

1. On the **Resources** page, click **associate with instance** in the appropriate address's **Instance** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-eip4.png)

1. On the address's details page, select the appropriate instance, specify the address's name, and click **Save** to associate the address with the instance\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-eip5.png)

**Note**  
If the Elastic IP address is currently associated with another online instance, AWS OpsWorks Stacks automatically reassigns the address to the new instance\.

You can also use the details page to move an associated Elastic IP address to another instance\.

**To move an Elastic IP address to another instance**

1. On the **Resources** page, click **Elastic IPs** and click **edit** in the address's **Actions** column\.

1. Do one of the following:
   + To move the address to another instance in the stack, select the appropriate instance from the **Instance** list and click **Save**\.
   + To move the address to an instance in another stack, click **change** in the **Stack** settings to see a list of the available stacks\. Select a stack from the **Stack** list and an instance from the **Instance** list\. Then click **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-eip7.png)

After you attach or move an address, AWS OpsWorks Stacks triggers a [Configure lifecycle event](workingcookbook-events.md) to notify the stack's instances of the change\.

## Attaching Amazon RDS Instances to an App<a name="resources-attach-rds"></a>

You can attach an Amazon RDS instance to one or more apps\.

**To attach an Amazon RDS instance to an app**

1. On the **Resources** page, click **Add app** in the appropriate instance's **Apps** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-rds4.png)

1. Use the **Add App** page to attach the Amazon RDS instance\. For more information, see [Adding Apps](workingapps-creating.md)\.

Because an Amazon RDS can be attached to multiple apps, there is no special procedure for moving the instance from one app to another\. Just edit the first app to remove the RDS instance or edit the second app to add the RDS instance\. For more information, see [Editing Apps](workingapps-editing.md)\.