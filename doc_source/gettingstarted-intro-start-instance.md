# Step 3: Start the Instance and Deploy the App<a name="gettingstarted-intro-start-instance"></a>

Now that you have an instance and an app, start the instance and deploy the app to the instance\.

**To start the instance and deploy the app**

1. Do one of the following:
   + In the service navigation pane, choose **Instances**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-nav-pane-console.png)
   + On the **My Sample Stack \(Linux\)** page, choose **Instances**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-instances-console.png)

1. On the **Instances** page, for **Node\.js App Server**, for **nodejs\-server1**, choose **start**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-start-instance-console.png)

1. Do not proceed until the **online** circle is bright green\. \(If you see a failure message, consult the [Debugging and Troubleshooting Guide](troubleshoot.md)\.\)

1. As the instance is setting up, AWS OpsWorks Stacks deploys the app to the instance\.

1. Your results should resemble the following screenshot before you continue \(if you receive a failure message, you may want to consult the [Debugging and Troubleshooting Guide](troubleshoot.md)\.\):  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-example-instance-started-console.png)

You now have an instance with an app that has been deployed to the instance\.

In the [next step](gettingstarted-intro-test-app.md), you test the app on the instance\.