# Step 4: Assign the Recipe to a LifeCycle Event<a name="other-services-redis-event"></a>

You can run custom recipes [manually](workingcookbook-manual.md), but the best approach is usually to have AWS OpsWorks Stacks run them automatically\. Every layer has a set of built\-in recipes assigned each of five [lifecycle events](workingcookbook-events.md)—Setup, Configure, Deploy, Undeploy, and Shutdown\. Each time an event occurs for an instance, AWS OpsWorks Stacks runs the associated recipes for each of the instance's layers, which handle the corresponding tasks\. For example, when an instance finishes booting, AWS OpsWorks Stacks triggers a Setup event\. This event runs the associated layer's Setup recipes, which typically handle tasks such as installing and configuring packages\.

You can have AWS OpsWorks Stacks run a custom recipe on a layer's instances by assigning the recipe to the appropriate lifecycle event\. For this example, you should assign the `generate.rb` recipe to the Rails App Server layer's Deploy event\. AWS OpsWorks Stacks will then run it on the layer's instances during startup, after the Setup recipes have finished, and every time you deploy an app\. For more information, see [Automatically Running Recipes](workingcookbook-assigningcustom.md)\.

**To assign a recipe to the Rails App Server layer's Deploy event**

1. On the AWS OpsWorks Stacks **Layers** page, for Rails App Server, click **Recipes** and then click **Edit**\.\.

1. Under **Custom Chef Recipes**, add the fully qualified recipe name to the deploy event and click **\+**\. A fully qualified recipe name uses the `cookbookname::recipename `format, where `recipename` does not include the `.rb` extension\. For this example, the fully qualified name is `redis-config::generate`\. Then click **Save** to update the layer configuration\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/redis_walkthrough_event.png)