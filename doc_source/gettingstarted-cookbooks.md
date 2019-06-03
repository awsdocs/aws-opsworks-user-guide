# Getting Started with Cookbooks in AWS OpsWorks Stacks<a name="gettingstarted-cookbooks"></a>

A production\-level AWS OpsWorks Stacks stack typically requires some customization, which often means implementing a custom Chef cookbook\. A *cookbook* is a package file that contains configuration information, including instructions called *recipes*\. A *recipe* is a set of one or more instructions, written with Ruby language syntax, that specifies the resources to use and the order in which those resources are applied\. A *resource*, as used in Chef, is a statement of configuration policy\. This walkthrough provides a basic introduction to implementing Chef cookbooks for AWS OpsWorks Stacks\. To learn more about Chef, cookbooks, recipes, and resources, see the links in [Next Steps](gettingstarted-cookbooks-next-steps.md)\.

This walkthrough mostly describes how to create your own cookbooks\. You can also use community\-provided cookbooks available on websites like the [Chef Supermarket](https://supermarket.chef.io)\. To help you get started with community cookbooks, we include instructions for using a community cookbook from the Chef Supermarket later in the walkthrough\.

Before you start this walkthrough, complete a few setup steps\. If you have already completed any of the other walkthroughs in this chapter, such as [Getting Started: Sample](gettingstarted-intro.md), then you have met the prerequisites for this walkthrough and can skip to [start this walkthrough](gettingstarted-cookbooks-create-cookbook.md)\. Otherwise, be sure to complete the [prerequisites](gettingstarted-intro-prerequisites.md), and then return to this walkthrough\.

**Topics**
+ [Step 1: Create the Cookbook](gettingstarted-cookbooks-create-cookbook.md)
+ [Step 2: Create the Stack and its Components](gettingstarted-cookbooks-create-stack.md)
+ [Step 3: Run and Test the Recipe](gettingstarted-cookbooks-test-recipe.md)
+ [Step 4: Update the Cookbook to Install a Package](gettingstarted-cookbooks-install-package.md)
+ [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md)
+ [Step 6: Update the Cookbook to Add a User](gettingstarted-cookbooks-add-user.md)
+ [Step 7: Update the Cookbook to Create a Directory](gettingstarted-cookbooks-create-directory.md)
+ [Step 8: Update the Cookbook to Create and Copy Files](gettingstarted-cookbooks-create-file.md)
+ [Step 9: Update the Cookbook to Run a Command](gettingstarted-cookbooks-run-command.md)
+ [Step 10: Update the Cookbook to Run a Script](gettingstarted-cookbooks-run-script.md)
+ [Step 11: Update the Cookbook to Manage a Service](gettingstarted-cookbooks-manage-service.md)
+ [Step 12: Update the Cookbook to Use Custom JSON](gettingstarted-cookbooks-custom-json.md)
+ [Step 13: Update the Cookbook to Use Data Bags](gettingstarted-cookbooks-data-bags.md)
+ [Step 14: Update the Cookbook to Use Iteration](gettingstarted-cookbooks-iteration.md)
+ [Step 15: Update the Cookbook to Use Conditional Logic](gettingstarted-cookbooks-conditional-logic.md)
+ [Step 16: Update the Cookbook to Use Community Cookbooks](gettingstarted-cookbooks-community-cookbooks.md)
+ [Step 17: \(Optional\) Clean Up](gettingstarted-cookbooks-clean-up.md)
+ [Next Steps](gettingstarted-cookbooks-next-steps.md)