# Step 7: Explore the Instance's Settings and Logs<a name="gettingstarted-intro-explore-instance"></a>

Examine the settings that AWS OpsWorks Stacks used to launch the instance\. You can also examine the instance logs that AWS OpsWorks Stacks created\.

**To display the instance's settings and logs**

1. In the service navigation pane, choose **Instances**\. The **Instances** page displays\.

1. For **Node\.js App Server**, choose **nodejs\-server1**\. The instance's properties page is shown\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-instance-details-page-console.png)

1. To explore the instance logs, in the **Logs** section, for **Log**, choose **show**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-instance-details-logs-console.png)

1. AWS OpsWorks Stacks displays the log in a separate web browser tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-instance-log-console.png)

To learn more about what some of the instance settings represent, return to the **nodejs\-server1** page, choose **Stop**, and when you see the confirmation message, choose **Stop**\. Choose **Edit** after **Status** changes from **stopping** to **stopped**, and then hover over each of the settings\. \(Not all settings have on\-screen descriptions\.\) For more information about these settings, see [Adding an Instance to a Layer](workinginstances-add.md)\.

When you have finished reviewing settings, choose **Start** to restart the instance, and wait until **Status** changes to **online**\. Otherwise, you won't be able to test the app later, because the instance will remain stopped\.

**Note**  
If you want to log in to the instance to explore it further, you must first provide AWS OpsWorks Stacks with information about your public SSH key \(which you can create with tools such as ssh\-keygen or PuTTYgen\), and then you must set permissions on the **My Sample Stack \(Linux\)** stack to enable your IAM user to log in to the instance\. For instructions, see [Registering an IAM User's Public SSH Key](security-settingsshkey.md) and [Logging In with SSH](workinginstances-ssh.md)\.

In the [next step](gettingstarted-intro-explore-app.md), explore the app's settings\.