# Step 3\.4: Run the Recipes<a name="gettingstarted-db-lifecycle"></a>

After you have your custom cookbook, you need to run the recipes on the appropriate instances\. You could [run them manually](workingcookbook-manual.md)\. However, recipes typically need to be run at predictable points in an instance's lifecycle, such as after the instance boots or when you deploy an app\. This section describes a much simpler approach: have AWS OpsWorks Stacks automatically run them for you at the appropriate time\.

AWS OpsWorks Stacks supports a set of [lifecycle events](workingcookbook-events.md) that simplify running recipes\. For example, the Setup event occurs after an instance finishes booting and the Deploy event occurs when you deploy an app\. Each layer has a set of built\-in recipes associated with each lifecycle event\. When a lifecycle event occurs on an instance, the agent runs the associated recipes for each of the instance's layers\. To have AWS OpsWorks Stacks run a custom recipe automatically, add it to the appropriate lifecycle event on the appropriate layer and the agent will run the recipe after the built\-in recipes are finished\.

For this example, you need to run two recipes, `dbsetup.rb` on the MySQLinstance and `appsetup.rb` on the PHP App Server instance\.

**Note**  
You specify recipes on the console by using the *cookbook\_name*::*recipe\_name* format, where *recipe\_name* does not include the \.rb extension\. For example, you refer to `dbsetup.rb` as **phpapp::dbsetup**\.

**To assign custom recipes to lifecycle events**

1. On the **Layers** page, for MySQL, click **Recipes** and then click **Edit**\.

1.  In the **Custom Chef recipes** section, enter [**phpapp::dbsetup**](gettingstarted-db-recipes.md#gettingstarted-db-recipes-dbsetup) for **Deploy**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gsb6a.png)

1. Click the **\+** icon to assign the recipe to the event and click **Save** to save the new layer configuration\.

1. Return to the **Layers** page and repeat the procedure to assign **phpapp::appsetup** to the **PHP App Server** layer's **Deploy** event\.