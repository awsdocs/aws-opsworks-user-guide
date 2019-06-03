# Step 5: Launch an Instance<a name="gettingstarted-linux-launch-instance"></a>

Use AWS OpsWorks Stacks to start up an Ubuntu Server Amazon EC2 instance\. This instance uses the settings that you defined in the layer you created earlier in this walkthrough\. \(For more information, see [Instances](workinginstances.md)\.\)

**To launch the instance**

1. In the service navigation pane, choose **Instances**\. The **Instances** page is displayed\.

1. For **MyLinuxDemoLayer**, choose **Add an instance**\.

1. On the **New** tab, leave the defaults for the following:
   + **Hostname** \(**demo1**\)
   + **Size** \(**c3\.large**\)
   + **Subnet** \(*IP address* **us\-west\-2a**\)

1. Choose **Advanced**\.

1. Leave the defaults for the following:
   + **Scaling type** \(**24/7**\)
   + **SSH key** \(**Do not use a default SSH key**\)
   + **Operating system** \(**Ubuntu 18\.04 LTS**\)
   + **OpsWorks Agent version** \(**Inherit from stack**\)
   + **Tenancy** \(**Default \- Rely on VPC settings**\)
   + **Root device type** \(**EBS backed**\)
   + **Volume type** \(**General Purpose \(SSD\)**\)
   + **Volume size** \(**8**\)

1. Your results will be similar to the following screenshot:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-add-instance-console.png)

1. Choose **Add Instance**\. AWS OpsWorks Stacks adds the instance to the layer and displays the **Instances** page\.

1. For **MyLinuxDemoLayer**, for **demo1**, for **Actions**, choose **start**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-start-instance-console.png)

1. Over the course of several minutes, the following occurs:
   + The **setting up** circle changes from **0** to **1**\. 
   + **Status** turns from **stopped** to **requested**, to **pending**, to **booting**, to **running\_setup**, and then finally to **online**\. Note that this process can take several minutes\.
   + After **Status** changes to **online**, the **setting up** circle indicator changes from **1** to **0**, and the **online** circle changes from **0** to **1** and changes to bright green\. Do not proceed until the **online** circle changes to bright green, and shows **1** instance online\. 

1. Your results must match the following screenshot before you continue \(if you receive a failure message, you may want to consult the [Debugging and Troubleshooting Guide](troubleshoot.md)\):  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-instance-started-console.png)

You now have an instance that is ready for the app to be deployed to it\. 

**Note**  
If you want to log in to the instance to explore it further, you must first provide AWS OpsWorks Stacks with information about your public SSH key \(which you can create with tools such as ssh\-keygen or PuTTYgen\), and then you must set permissions on the `MyLinuxDemoStack` stack to enable your IAM user to log in to the instance\. For instructions, see [Registering an IAM User's Public SSH Key](security-settingsshkey.md) and [Logging In with SSH](workinginstances-ssh.md)\. If you plan to use SSH to connect to instances through PuTTY, see [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the AWS documentation\.

In the [next step](gettingstarted-linux-deploy-app.md), you will deploy the app to the instance\.