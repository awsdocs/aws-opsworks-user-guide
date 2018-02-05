# Automatically Running Recipes<a name="workingcookbook-assigningcustom"></a>

Each layer has a set of built\-in recipes assigned to each lifecycle event, although some layers lack Undeploy recipes\. When a lifecycle event occurs on an instance, AWS OpsWorks Stacks runs the appropriate set of recipes for the associated layer\.

If you have installed custom cookbooks, you can have AWS OpsWorks Stacks run some or all of the recipes automatically by assigning each recipe to a layer's lifecycle event\. After an event occurs, AWS OpsWorks Stacks runs the specified custom recipes after the layer's built\-in recipes\. 

**To assign custom recipes to layer events**

1. On the **Layers** page, for the appropriate layer, click **Recipes** and then click **Edit**\. If you haven't yet enabled custom cookbooks, click **configure cookbooks** to open the stack's **Settings** page\. Toggle **Use custom Chef Cookbooks** to **Yes**, and provide the cookbook's repository information\. Then click **Save** and navigate back to the edit page for the **Recipes** tab\. For more information, see [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)\.

1. On the **Recipes** tab, enter each custom recipe in the appropriate event field and click **\+** to add it to the list\. Specify a recipe as follows: *cookbook*::*somerecipe* \(omit the `.rb` extension\)\.   
![\[Layer details page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_edit.png)

When you start a new instance, AWS OpsWorks Stacks automatically runs the custom recipes for each event, after it runs the standard recipes\.

**Note**  
Custom recipes execute in the order that you enter them in the console\. An alternative way to control execution order is to implement a meta recipe that executes the recipes in the correct order\.