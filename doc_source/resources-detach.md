# Detaching Resources<a name="resources-detach"></a>

When you no longer need an attached resource, you can detach it\. This resource remains registered with the stack and can be attached elsewhere\. 

**Topics**
+ [Unassigning Amazon EBS Volumes](#resources-detach-ebs)
+ [Disassociating Elastic IP Addresses](#resources-detach-eip)
+ [Detaching Amazon RDS Instances](#resources-detach-rds)

## Unassigning Amazon EBS Volumes<a name="resources-detach-ebs"></a>

Use the following procedure to unassign an Amazon EBS volume from its instance\.

**To unassign an Amazon EBS volume**

1. Ensure that the instance is in the offline state\.

1. On the **Resources** page, click **Volumes** and click volume name\.

1. On the volume's details page, click **Unassign**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-ebs8.png)

## Disassociating Elastic IP Addresses<a name="resources-detach-eip"></a>

Use the following procedure to disassociate an Elastic IP address from its instance\.

**To disassociate an Elastic IP address**

1. On the **Resources** page, click **Elastic IPs** and click **edit** in the address's **Actions** column\.

1. On the address's details page, click **Disassociate**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/resources-eip8.png)

After you disassociate an address, AWS OpsWorks Stacks triggers a [Configure lifecycle event](workingcookbook-events.md) to notify the stack's instances of the change\.

## Detaching Amazon RDS Instances<a name="resources-detach-rds"></a>

Use the following procedure to detach an Amazon RDS from an app\.

**To detach an Amazon RDS instance**

1. On the **Resources** page, click **RDS** and click the appropriate app in the **Apps** column\.

1. Click **Edit** and edit the app configuration to detach the instance\. For more information, see [Editing Apps](workingapps-editing.md)\.

**Note**  
This procedure detaches an Amazon RDS from a single app\. If the instance is attached to multiple apps, you must repeat this procedure for each app\.