# Manually Running Recipes<a name="workingcookbook-manual"></a>

Although recipes typically are  run automatically in response to lifecycle events, you can manually run recipes at any time on any or all stack instances\. This feature is typically used for tasks that don't map well to a lifecycle event, such as backing up instances\. To run a custom recipe manually, it must be in one of your custom cookbooks, but it does not have to be assigned to a lifecycle event\. When you run a recipe manually, AWS OpsWorks Stacks installs the same `deploy` attributes that it does for a Deploy event\.

**To manually run recipes on stack instances**

1. On the **Stack** page, click **Run command**\. For **Command**, select **Execute Recipes**\.  
![\[Execute Recipes command on the Run command page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/execute_recipe.png)

1. Enter the recipes to be run in the** Recipes to execute** box by using the standard *cookbookname*::*recipename* format\. Use commas to separate multiple recipes; they will run in the order that you list them\.

1. Optionally, use the **Custom Chef JSON** box to add a custom JSON object that defines custom attributes that will be merged into the stack configuration and deployment attributes that are installed on the instances\. For more information about using custom JSON objects, see [Using Custom JSON](workingstacks-json.md) and [Overriding Attributes](workingcookbook-attributes.md)\.

1. Under **Instances**, select the instances on which AWS OpsWorks Stacks should run the recipes\. 

When a lifecycle event occurs, the AWS OpsWorks Stacks agent receives a command to run the associated recipes\. You can manually run these commands on a particular instance by using the appropriate [stack command](workingstacks-commands.md) or by using the agent CLI's [ run\_command](agent-run.md) command\. For more information on how to use the agent CLI, see [AWS OpsWorks Stacks Agent CLI](agent.md)\.