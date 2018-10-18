# Step 4: Specify the App to Deploy to the Instance<a name="gettingstarted-linux-specify-app"></a>

Tell AWS OpsWorks Stacks about the app that you will deploy to the instance later in this walkthrough\. In this context, AWS OpsWorks Stacks defines an *app* as code you want to run on an instance\. \(For more information, see [Apps](workingapps.md)\.\)

The procedure in this section applies to Chef 12 and newer stacks\. For information about how to add apps to layers in Chef 11 stacks, see [Step 2\.4: Create and Deploy an App \- Chef 11](gettingstarted-simple-app.md)\.

**To specify the app to deploy**

1. In the service navigation pane, choose **Apps**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-nav-pane-console.png)

1. The **Apps** page is displayed\. Choose **Add an app**\. The **Add App** page is displayed\.

1. For **Settings**, for **Name**, type **MyLinuxDemoApp**\. \(You can type a different name, but be sure to substitute it for `MyLinuxDemoApp` throughout this walkthrough\.\)

1. For **Application Source**, for **Repository URL**, type **https://github\.com/awslabs/opsworks\-windows\-demo\-nodejs\.git**

1. Leave the defaults for the following:
   + **Settings**, **Document root** \(blank\)
   + **Data Sources**, **Data source type** \(**None**\)
   + **Repository type** \(**Git**\)
   + **Repository SSH key** \(blank\)
   + **Branch/Revision** \(blank\)
   + **Environment Variables** \(blank **KEY**, blank **VALUE**, unchecked **Protected Value**\)
   + **Add Domains**, **Domain Name** \(blank\)
   + **SSL Settings**, **Enable SSL** \(**No**\)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-add-app-top-console.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-add-app-bottom-console.png)

1. Choose **Add App**\. AWS OpsWorks Stacks adds the app and displays the **Apps** page\.

You now have an app with the correct settings for this walkthrough\.

In the [next step](gettingstarted-linux-launch-instance.md), you will launch the instance\.