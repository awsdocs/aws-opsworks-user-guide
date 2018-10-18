# Step 3: Run and Test the Recipe<a name="gettingstarted-cookbooks-test-recipe"></a>

Run and test the `default` recipe from within the cookbook that AWS OpsWorks Stacks copied to the instance\. As you'll recall, this is the one\-line recipe that displays a simple message in the log when the recipe runs\.

**To run the recipe**

1. In the service navigation pane, choose **Stack**\. The **MyCookbooksDemoStack** page is displayed\.

1. Choose **Run Command**\. The **Run Command** page is displayed\.

1. For **Command**, choose **Execute Recipes**\.

1. For **Recipes to execute**, type **opsworks\_cookbook\_demo::default**\.

   **opsworks\_cookbook\_demo** is the name of the cookbook as defined in the `metadata.rb` file\. **default** is the name of the recipe to run, that is, the name of the `default.rb` file in the cookbook's `recipes` subdirectory, without the file extension\.

1. Leave the following default settings:
   + **Comment** \(blank\)
   + **Advanced**, **Custom Chef JSON** \(blank\)
   + **Instances** \(**Select all** checked, **MyCookbooksDemoLayer** checked, **cookbooks\-demo1** checked\)

1. Choose **Execute Recipes**\. The **Running command execute\_recipes** page is displayed\. Do not proceed until **Status** changes to **successful**\. This process might take a few minutes, so be patient\.

**To check the recipe's results**

1. With the **Running command execute\_recipes** page displayed, for **cookbooks\-demo1**, for **Log**, choose **show**\. The **execute\_recipes** log page is displayed\.

1. Scroll down the log and find an entry that looks similar to the following:

   ```
   [2015-11-13T19:14:39+00:00] INFO: ********** Hello, World! **********
   ```

You have successfully run your first recipe\! In the [next step](gettingstarted-cookbooks-install-package.md), you will update your cookbook by adding a recipe that installs a package on the instance\.