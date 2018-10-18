# Migrating an Existing Linux Stack to a new Chef Version<a name="workingcookbook-chef11-migrate"></a>

You can use AWS OpsWorks Stacks console, API, or CLI to migrate your Linux stacks to a newer Chef version\. However, your recipes might require modification to be compatible with the newer version\. When preparing to migrate a stack, consider the following\.
+ You cannot change AWS OpsWorksStacks stack versions from Chef 11 to Chef 12 by editing or cloning the stack\. A Chef major version upgrade cannot be performed using the procedure in this section\. For more information on the Chef 11\.10 to Chef 12 transition, see [Implementing Recipes: Chef 12](workingcookbook-chef12-linux.md)\.
+ The transition from one Chef version to another involves a number of changes, some of them breaking changes\.

  For more information on the Chef 0\.9 to Chef 11\.4 transition, see [Migrating to a new Chef Version](#workingcookbook-chef11-migrate)\. For more information on the Chef 11\.4 to Chef 11\.10 transition, see [Implementing Recipes: Chef 11\.10](workingcookbook-chef11-10.md)\. For more information on the Chef 11\.10 to Chef 12 transition, see [Implementing Recipes: Chef 12](workingcookbook-chef12-linux.md)\.
+ Chef runs use a different Ruby version on Chef 0\.9 and Chef 11\.4 stacks \(Ruby 1\.8\.7\), Chef 11\.10 stacks \(Ruby 2\.0\.0\), and Chef 12 stacks \(Ruby 2\.1\.6\)\.

  For more information, see [Ruby Versions](workingcookbook-ruby.md)\.
+ Chef 11\.10 stacks handle cookbook installation differently from Chef 0\.9 or Chef 11\.4 stacks\.

  This difference could cause problems when migrating stacks that use custom cookbooks to Chef 11\.10\. For more information, see [Cookbook Installation and Precedence](workingcookbook-chef11-10.md#workingcookbook-chef11-10-override)\.

 The following are recommended guidelines for migrating a Chef stack to a newer Chef version:

**To migrate a stack to a newer Chef version**

1. [Clone your production stack](workingstacks-cloning.md)\. On the **Clone Stack** page, click **Advanced>>** to display the **Configuration Management** section, and change **Chef version** to the next higher version\.
**Note**  
If you are starting with a Chef 0\.9 stack, you cannot upgrade directly to Chef 11\.10; you must first upgrade to Chef 11\.4\. If you want to migrate your stack to Chef 11\.10 before testing your recipes, wait 20 minutes for the update to be executed, and then upgrade the stack from 11\.4 to 11\.10\.

1. Add instances to the layers and test the cloned stack's applications and cookbooks on a testing or staging system\. For more information, see [All about Chef \.\.\.](https://docs.chef.io/index.html)\.

1. When the test results are satisfactory, do one of the following:
   + If this is your desired Chef version, you can use the cloned stack as your production stack, or reset the Chef version on your production stack\. 
   + If you are migrating a Chef 0\.9 stack to Chef 11\.10 in two stages, repeat the process to migrate the stack from Chef 11\.4 to Chef 11\.10\.

**Note**  
When you are testing recipes, you can [use SSH to connect to](workinginstances-ssh.md) the instance and then use the [Instance Agent CLI](agent.md) [run\_command](agent-run.md) command to run the recipes associated with the various lifecycle events\. The agent CLI is especially useful for testing Setup recipes because you can use it even Setup fails and the instance does not reach the online state\. You can also use the [Setup stack command](workingstacks-commands.md) to rerun Setup recipes, but that command is only available if Setup succeeded and the instance is online\. 

It is possible to update a running stack to a new Chef version\.

**To update a running stack to a new Chef version**

1. [Edit the stack](workingstacks-edit.md) to change the **Chef version** stack setting\.

1. Save the new settings and wait for AWS OpsWorks Stacks to update the instances, which typically takes 15 \- 20 minutes\.

**Important**  
AWS OpsWorks Stacks does not synchronize the Chef version update with lifecycle events\. If you want to update the Chef version on a production stack, you must take care to ensure that the update is complete before the next [lifecycle event](workingcookbook-events.md) occurs \. If an event occurs—typically a Deploy or Configure event—the instance agent updates your custom cookbooks and runs the event's assigned recipes, whether the version update is complete or not\. There is no direct way to determine when the version update is complete, but deployment logs include the Chef version\.