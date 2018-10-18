# Deregistering Resources<a name="resources-dereg"></a>

If you no longer need to have a resource registered with a stack, you can deregister it\. Deregistration does not delete the resource from your account; it remains there and can be registered with another stack or used outside AWS OpsWorks Stacks\. If you want to delete the resource entirely, you have two options:
+ If an Elastic IP or Amazon EBS resource is attached to an instance, you can delete the resource when you delete the instance\.

  Go to the **Instances** page, click **delete** in the instance's **Actions** column, and then select **Delete instance's EBS volumes** or **Delete the instance's Elastic IP**\. 
+ Deregister the resource and then use the Amazon EC2 or Amazon RDS console, API, or CLI to delete it\.

**Topics**
+ [Deregistering Amazon EBS Volumes](#resources-dereg-ebs)
+ [Deregistering Elastic IP Addresses](#resources-dereg-eip)
+ [Deregistering Amazon RDS Instances](#resources-dereg-rds)

## Deregistering Amazon EBS Volumes<a name="resources-dereg-ebs"></a>

Use the following procedure to deregister an Amazon EBS volume\.

**To deregister an Amazon EBS volume**

1. If the volume is attached to an instance, unassign it, as described in [Unassigning Amazon EBS Volumes](resources-detach.md#resources-detach-ebs)\.

1. On the **Resources** page, click the volume name in the **Name** column\.

1. On the volume's details page, click **Deregister**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-ebs9.png)

## Deregistering Elastic IP Addresses<a name="resources-dereg-eip"></a>

Use the following procedure to deregister an Elastic IP address\.

**To deregister an Elastic IP address**

1. If the address is associated with an instance, disassociate it, as described in [Disassociating Elastic IP Addresses](resources-detach.md#resources-detach-eip)\.

1. On the **Resources** page, click **Elastic IPs** and then click the IP address in the **Address** column\.

1. On the address's details page, click **Deregister**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-eip9.png)

**Note**  
If you simply want to register an Elastic IP address with a different stack, you must deregister it from its current stack and then register it with the new stack\. However, you can move an attached Elastic IP address to an instance in another stack directly\. For more information, see [Attaching and Moving Resources](resources-attach.md)\.

## Deregistering Amazon RDS Instances<a name="resources-dereg-rds"></a>

Use the following procedure to deregister an Amazon RDS instance\.

**To deregister an Amazon RDS instance**

1. If the instance is associated with an app, detach it, as described in [Detaching Resources](resources-detach.md)\.

1. On the **Resources** page, click **RDS** and then instance's name\.

1. On the instance's details page, click **Deregister**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-rds5.png)