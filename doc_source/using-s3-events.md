# Step 4: Assign the Recipes to LifeCycle Events<a name="using-s3-events"></a>

You can run custom recipes [manually](workingcookbook-manual.md), but the best approach is usually to have AWS OpsWorks Stacks run them automatically\. Every layer has a set of built\-in recipes assigned to each of five [lifecycle events](workingcookbook-events.md)—Setup, Configure, Deploy, Undeploy, and Shutdown—\. Each time an event occurs on an instance, AWS OpsWorks Stacks runs the associated recipes for each of the instance's layers, which handle the required tasks\. For example, when an instance finishes booting, AWS OpsWorks Stacks triggers a Setup event to run the Setup recipes, which typically handle tasks such as installing and configuring packages \.

You can have AWS OpsWorks Stacks run custom recipes on a layer's instances by assigning each recipe to the appropriate lifecycle event\. AWS OpsWorks Stacks will run any custom recipes after the layer's built\-in recipes have finished\. For this example, assign `appsetup.rb` to the PHP App Server layer's Deploy event and `dbsetup.rb` to the MySQL layer's Deploy event\. AWS OpsWorks Stacks will then run the recipes on the associated layer's instances during startup, after the built\-in Setup recipes have finished, and every time you deploy an app, after the built Deploy recipes have finished\. For more information, see [Automatically Running Recipes](workingcookbook-assigningcustom.md)\.

**To assign custom recipes to the layer's Deploy event**

1. On the AWS OpsWorks Stacks **Layers** page, for the PHP App Server choose **Recipes** and then choose **Edit**\. 

1. Under **Custom Chef Recipes**, add the recipe name to the deploy event and choose **\+**\. The name must be in the Chef `cookbookname::recipename` format, where `recipename` does not include the `.rb` extension\. For this example, you enter `photoapp::appsetup`\. Then choose **Save** to update the layer configuration\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/psb6a.png)

1. On the **Layers** page, choose **edit** in the MySQL layer's **Actions** column\.

1. Add `photoapp::dbsetup` to the layer's Deploy event and save the new configuration\.