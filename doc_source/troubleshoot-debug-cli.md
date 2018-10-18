# Using the AWS OpsWorks Stacks Agent CLI<a name="troubleshoot-debug-cli"></a>

**Note**  
The agent CLI is available only on Linux instances\.

On every online instance, AWS OpsWorks Stacks installs an agent, which communicates with the service\. The AWS OpsWorks Stacks service in turn sends commands to the agent to performs tasks such as initiating Chef runs on the instance when a lifecycle event occurs\. On Linux instances, the agent exposes a command line interface \(CLI\) that is very useful for troubleshooting\. To run agent CLI commands, use [SSH to connect to an instance](workinginstances-ssh.md)\. You can then run agent CLI commands to perform a variety of tasks, including the following: 
+ Execute recipes\.
+ Display Chef logs\.
+ Display [stack configuration and deployment JSON](workingcookbook-json.md)\.

For more information on how to set up an SSH connection to an instance, see [Logging In with SSH](workinginstances-ssh.md)\. You must also have [SSH and sudo permissions](opsworks-security-users.md) for the stack\.

This section describes how to use the agent CLI for troubleshooting\. For more information and a complete command reference, see [AWS OpsWorks Stacks Agent CLI](agent.md)\.

**Topics**
+ [Executing Recipes](#troubleshoot-debug-cli-recipes)
+ [Displaying Chef Logs](#troubleshoot-debug-cli-log)
+ [Displaying the Stack Configuration and Deployment JSON](#troubleshoot-debug-cli-json)

## Executing Recipes<a name="troubleshoot-debug-cli-recipes"></a>

The agent CLI [`run_command`](agent-run.md) command directs the agent to rerun a command that it performed earlier\. The most useful commands for troubleshooting—`setup`, `configure`, `deploy`, and `undeploy`—each correspond to a lifecycle event\. They direct the agent to initiate a Chef run to execute the associated recipes\.

**Note**  
The `run_command` command is limited to executing the group of recipes that is associated with a specified command, typically the recipes that are associated with a lifecycle event\. You cannot use it to execute a particular recipe\. To execute one or more specified recipes, use the [Execute Recipes stack command](workingstacks-commands.md) or the equivalent CLI or API actions \([http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-deployment.html](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-deployment.html) and [http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateDeployment.html](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateDeployment.html)\)\.

The `run_command` command is quite useful for debugging custom recipes, especially recipes that are assigned to the Setup and Configure lifecycle events, which you can't trigger directly from the console\. By using `run_command`, you can run a particular event's recipes as often as you need without having to start or stop instances\.

**Note**  
AWS OpsWorks Stacks runs recipes from the instance's cookbook cache, not the cookbook repository\. AWS OpsWorks Stacks downloads cookbooks to this cache when the instance starts, but does not automatically update the cache on online instances if you subsequently modify your cookbooks\. If you have modified your cookbooks since starting the instance, be sure to run the [Update Cookbooks stack command](workingstacks-commands.md) stack command to update the cookbook cache with the most recent version from the repository\.

The agent caches only the most recent commands\. You can list them by running [`list_commands`](agent-list.md), which returns a list of cached commands and the time that they were performed\.

```
sudo opsworks-agent-cli list_commands
2013-02-26T19:08:26        setup
2013-02-26T19:12:01        configure
2013-02-26T19:12:05        configure
2013-02-26T19:22:12        deploy
```

To rerun the most recent command, run this:

```
sudo opsworks-agent-cli run_command
```

To rerun the most recent instance of a specified command, run this:

```
sudo opsworks-agent-cli run_command command
```

For example, to rerun the Setup recipes, you can run the following command:

```
sudo opsworks-agent-cli run_command setup
```

Each command has an associated [stack configuration and deployment JSON](workingcookbook-json.md) that represents stack and deployment state at the time the command was run\. Because that data can change from one command to the next, an older instance of a command might use somewhat different data than the most recent one\. To rerun a particular instance of a command, copy the time from the `list_commands` output and run the following:

```
sudo opsworks-agent-cli run_command time
```

The preceding examples all rerun the command using the default JSON, which is the JSON was installed for that command\. You can rerun a command against an arbitrary JSON file as follows:

```
sudo opsworks-agent-cli run_command /path/to/valid/json.file
```

## Displaying Chef Logs<a name="troubleshoot-debug-cli-log"></a>

The agent CLI [`show_log`](agent-show.md) command displays a specified log\. After the command is finished, you will be looking at the end of the file\. The `show_log` command therefore provides a convenient way to tail the log, which is typically where you find error information\. You can scroll up to see the earlier parts of the log\.

To display the current command's log, run this:

```
sudo opsworks-agent-cli show_log
```

You can also display logs for a particular command, but be aware that the agent caches logs for only the last thirty commands\. You can list an instance's commands by running [`list_commands`](agent-list.md), which returns a list of cached commands and the time that they were performed\. For an example, see [Executing Recipes](#troubleshoot-debug-cli-recipes)\.

To show the log for the most recent execution of a particular command, run the following:

```
sudo opsworks-agent-cli show_log command
```

The command parameter can be set to `setup`, `configure`, `deploy`, `undeploy`, `start`, `stop`, or `restart`\. Most of these commands correspond to lifecycle events and direct the agent to run the associated recipes\.

To display the log for a particular command execution, copy the date from the `list_commands` output and run:

```
sudo opsworks-agent-cli show_log date
```

If a command is still executing, `show_log` displays the log's current state\.

**Note**  
One way to use `show_log` to troubleshoot errors and out\-of\-memory issues is to tail a log during execution, as follows:  
Use `run_command` to trigger the appropriate lifecycle event\. For more information, see [Executing Recipes](#troubleshoot-debug-cli-recipes)\.
Repeatedly run `show_log` to see the tail of the log as it is being written\.
If Chef runs out of memory or exits unexpectedly, the log will end abruptly\. If a recipe fails, the log will end with an exception and a stack trace\. 

## Displaying the Stack Configuration and Deployment JSON<a name="troubleshoot-debug-cli-json"></a>

Much of the data used by recipes comes from the [stack configuration and deployment JSON](workingcookbook-json.md), which defines a set of Chef attributes that provide a detailed description of the stack configuration, any deployments, and optional custom attributes that users can add\. For each command, AWS OpsWorks Stacks installs a JSON that represents the stack and deployment state at the time of the command \. For more information, see [Stack Configuration and Deployment Attributes](workingcookbook-json.md)\.

If your custom recipes obtain data from the stack configuration and deployment JSON, you can verify the data by examining the JSON\. The easiest way to display the stack configuration and deployment JSON is to run the agent CLI [`get_json`](agent-json.md) command, which displays a formatted version of the JSON object\. The following shows the first few lines of some typical output:

```
{
  "opsworks": {
    "layers": {
      "php-app": {
        "id": "4a2a56c8-f909-4b39-81f8-556536d20648",
        "instances": {
          "php-app2": {
            "elastic_ip": null,
            "region": "us-west-2",
            "booted_at": "2013-02-26T20:41:10+00:00",
            "ip": "10.112.235.192",
            "aws_instance_id": "i-34037f06",
            "availability_zone": "us-west-2a",
            "instance_type": "c1.medium",
            "private_dns_name": "ip-10-252-0-203.us-west-2.compute.internal",
            "private_ip": "10.252.0.203",
            "created_at": "2013-02-26T20:39:39+00:00",
            "status": "online",
            "backends": 8,
            "public_dns_name": "ec2-10-112-235-192.us-west-2.compute.amazonaws.com"
...
```

You can display the most recent stack configuration and deployment JSON as follows:

```
sudo opsworks-agent-cli get_json
```

You can display the most recent stack configuration and deployment JSON for a specified command by executing the following:

```
sudo opsworks-agent-cli get_json command
```

The command parameter can be set to `setup`, `configure`, `deploy`, `undeploy`, `start`, `stop`, or `restart`\. Most of these commands correspond to lifecycle events and direct the agent to run the associated recipes\.

You can display the stack configuration and deployment JSON for a particular command execution by specifying the command's date like this:

```
sudo opsworks-agent-cli get_json date
```

The simplest way to use this command is as follows:

1. Run `list_commands`, which returns a list of commands that have been run on the instance, and the date that each command was run\.

1. Copy the date for the appropriate command and use it as the `get_json` *date* argument\.