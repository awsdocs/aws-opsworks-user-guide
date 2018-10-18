# Creating an OpsWorks Layer<a name="workinglayers-basics-create"></a>

When you create a new stack, you see the following page:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/new_stack_page_layers.png)

**To add the first OpsWorks layer**

1. Click **Add a Layer**\.

1. On the **Add Layer** page, select the appropriate layer, which displays the layer's configuration options\.

1. Configure the layer appropriately and click **Add Layer** to add it to the stack\. The following sections describe how to configure the various layers\.
**Note**  
The **Add Layer** page displays only the more commonly used configuration settings for each layer\. You can specify additional settings by [editing the layer](workinglayers-basics-edit.md)\. 

1. Add instances to the layer and start them\. 
**Note**  
If an instance is a member of multiple layers, you must add it to all of them before you start the instance\. You cannot add an online instance to a layer\.

To add more layers, open the **Layers** page and click **\+ Layer** to open the **Add Layer** page\.

When you start an instance, AWS OpsWorks Stacks automatically runs the Setup and Deploy recipes for each of the instance's layers to install and configure the appropriate packages and deploy the appropriate applications\. You can [customize a layer's setup and configuration process](customizing.md) in a variety of ways, such as by assigning custom recipes to the appropriate lifecycle events\. AWS OpsWorks Stacks runs custom recipes after the standard recipes for each event\. For more information, see [Cookbooks and Recipes](workingcookbook.md)\.

The following layer\-specific sections describe how handle Steps 2 and 3 for the various AWS OpsWorks Stacks layers\. For more information how to add instances, see [Adding an Instance to a Layer](workinginstances-add.md)\.