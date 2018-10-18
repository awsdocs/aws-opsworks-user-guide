# Step 2\.2: Authorize RDP Access<a name="gettingstarted-windows-rdp"></a>

Now that you have created a stack, you will create a layer and add a Windows instance to the layer\. However, before you can do so, you must configure the stack to allow you to use RDP to connect to the custom layer's instances\. To do so, you must do the following:
+ Add an inbound rule to the security group that controls RDP access\.
+ Set your AWS OpsWorks Stacks permissions for this stack to allow RDP access\. 

When you create the first stack in a region, AWS OpsWorks Stacks creates a set of security groups\. They include one named something like `AWS-OpsWorks-RDP-Server`, which AWS OpsWorks Stacks attaches to all Windows instances to allow RDP access\. However, by default, this security group does not have any rules, so you must add an inbound rule to allow RDP access to your instances\.

**To allow RDP access**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/v2/), set it to the stack's region, and choose **Security Groups** from the navigation pane\.

1. Choose **AWS\-OpsWorks\-RDP\-Server**, choose the **Inbound** tab, and choose **Edit**\.

1. Choose **Add Rule** and specify the following settings:
   + **Type** – **RDP**\.
   + **Source** – The permissible source IP addresses\.

     You typically allow inbound RDP requests from your IP address or a specified IP address range \(typically your corporate IP address range\)\. For learning purposes, it's often sufficient to specify 0\.0\.0\.0/0, which allows RDP access from any IP address\.

The security group allows the instance to receive RDP connection requests, but that's only half the story\. An ordinary user will log into the instance using a password provided by AWS OpsWorks Stacks\. To have AWS OpsWorks Stacks generate that password, you must explicitly authorize RDP access for the user\.

**To authorize RDP for a user**

1. In the AWS OpsWorks Stacks dashboard, choose the **IISWalkthrough** stack\.

1. In the navigation pane for the stack, choose **Permissions**\.

1. On the Permissions page, choose **Edit**\.

1. In the list of users, select the checkbox for **SSH/RDP** for the IAM user to whom you want to grant necessary permissions\. If you want the user to also have administrator permissions, select **sudo/admin** as well\.  
![\[SSH and sudo permissions for users\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/permissions.png)

1. Choose **Save**\.

The user can then get a password and use it to log in to the instance, as described later\.

**Note**  
You also can log in as Administrator\. For more information, see [Logging in As Administrator](workinginstances-rdp.md#workinginstances-rdp-admin)\.