# Editing Apps<a name="workingapps-editing"></a>

You can modify an app's configuration by editing the app\. For example, if you are ready to deploy a new version, you can edit the app's AWS OpsWorks Stacks settings to use the new repository branch\. You must have Manage or Deploy permissions to edit an app's configuration\. For more information, see [Managing User Permissions](opsworks-security-users.md)\.

**To edit an app**

1. On the **Apps** page click the app name to open its details page\.

1. Click **Edit** to change the app's configuration\.
   + If you modify the app's name, AWS OpsWorks Stacks uses the new name to identify the app in the console\. 

     Changing the name does not change the associated short name\. The short name is set when you add the app to the stack and cannot be subsequently modified\.
   + If you have specified a protected environment variable, you cannot see or edit the value\. However, you can specify a new value by clicking **Update value**\.

1. Click **Save** to save the new configuration and then **Deploy App** to deploy the app\. 

Editing an app changes the settings with AWS OpsWorks Stacks, but does not affect the stack's instances\. When you first [deploy an app](workingapps-deploying.md), the Deploy recipes download the code and related files to the app server instances, which then run the local copy\. If you modify the app in the repository or change any other settings, you must deploy the app to install the updates on your app server instances, as follows\. AWS OpsWorks Stacks automatically deploys the current app version to new instances when they are started\. For existing instances, however, the situation is different: 
+ AWS OpsWorks Stacks automatically deploys the current app version to new instances when they are started\.
+ AWS OpsWorks Stacks automatically deploys the latest app version to offline instances, including [load\-based and time\-based instances](workinginstances-autoscaling.md), when they are restarted\.
+ You must manually deploy the updated app to online instances\.

For more information on how to deploy apps, see [Deploying Apps](workingapps-deploying.md)