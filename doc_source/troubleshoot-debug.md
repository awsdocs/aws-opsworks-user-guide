# Debugging Recipes<a name="troubleshoot-debug"></a>

When a lifecycle event occurs, or you run the [Execute Recipes stack command](workingstacks-commands.md), AWS OpsWorks Stacks issues a command to the [agent](troubleshoot-debug-cli.md) to initiate a [Chef Solo run](https://docs.chef.io/ctl_chef_solo.html) on the specified instances to execute the appropriate recipes, including your custom recipes\. This section describes some ways that you can debug failed recipes\.

**Topics**
+ [Logging in to a Failed Instance](troubleshoot-debug-login.md)
+ [Chef Logs](troubleshoot-debug-log.md)
+ [Using the AWS OpsWorks Stacks Agent CLI](troubleshoot-debug-cli.md)