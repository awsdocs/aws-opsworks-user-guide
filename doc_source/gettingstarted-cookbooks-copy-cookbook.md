# Step 5: Update the Cookbook on the Instance and Run the Recipe<a name="gettingstarted-cookbooks-copy-cookbook"></a>

Update the cookbook on the instance and then run the recipe from within the updated cookbook on the instance\. Throughout the rest of this walkthrough, you repeat this step every time you update the cookbook by adding a new recipe\.

**To update the cookbook on the instance**

1. In the service navigation pane, choose **Stack**\. The **MyCookbooksDemoStack** page is displayed\.

1. Choose **Run Command**\. The **Run Command** page is displayed\.

1. For **Command**, choose **Update Custom Cookbooks**\.

1. Leave the following default settings:
   + **Comment** \(blank\)
   + **Advanced**, **Custom Chef JSON** \(blank\)
   + **Advanced**, **Instances** \(**Select all** checked, **MyCookbooksDemoLayer** checked, **cookbooks\-demo1** checked\)

1. Choose **Update Custom Cookbooks**\. The **Running command update\_custom\_cookbooks** page is displayed\. Do not proceed until **Status** changes to **successful**\. This process might take several minutes, so be patient\.

**To run the recipe**

1. In the service navigation pane, choose **Stack**\. The **MyCookbooksDemoStack** page is displayed\.

1. Choose **Run Command**\. The **Run Command** page is displayed\.

1. For **Command**, choose **Execute Recipes**\.

1. For **Recipes to execute**, type the name of the recipe to run\. The first time you do this, the recipe is named **opsworks\_cookbook\_demo::install\_package**\.
**Note**  
As you repeat this procedure later, type the name of the cookbook \(**opsworks\_cookbook\_demo**\), followed by two colons \(**::**\), followed by the name of the recipe \(the recipe's file name, without the `.rb` file extension\)\.

1. Leave the following default settings:
   + **Comment** \(blank\)
   + **Advanced**, **Custom Chef JSON** \(blank\)
   + **Instances** **Select all** checked, **MyCookbooksDemoLayer** checked, **cookbooks\-demo1** checked\)

1. Choose **Execute Recipes**\. The **Running command execute\_recipes** page is displayed\. Do not proceed until **Status** changes to **successful**\. This process might take a few minutes, so be patient\.

**Note**  
You don't have to manually run recipes\. You can assign recipes to a layer's lifecycle events, such as the Setup and Configure events, and AWS OpsWorks Stacks will run those recipes automatically when the event occurs\. For more information, see [AWS OpsWorks Stacks Lifecycle Events](workingcookbook-events.md)\.

In the [next step](gettingstarted-cookbooks-add-user.md), you will update the cookbook to add a user to the instance\.