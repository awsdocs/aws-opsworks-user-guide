# AWS OpsWorks Stacks Agent CLI<a name="agent"></a>

**Note**  
This feature is available only on Linux instances\.

The agent that AWS OpsWorks Stacks installs on every instance exposes a command line interface \(CLI\)\. If you [use SSH to log in](workinginstances-ssh.md) to the instance, you can use the CLI to the following: 
+ Access log files for Chef runs\. 
+ Access AWS OpsWorks Stacks commands\.
+ Manually run Chef recipes\.
+ View instance reports\.
+ View agent reports\.
+ View a limited set of stack configuration and deployment attributes\. 

**Important**  
You can run agent CLI commands only as root or by using `sudo`\.

The basic command syntax is:

```
sudo opsworks-agent-cli [--help] [command [activity] [date]]
```

The four arguments are as follows:

**help**  
\(Optional\) Displays a brief synopsis of the available commands when used by itself\. When used with a command, `help` displays a description of the command\.

**command**  
\(Optional\) The agent CLI command, which must be set to one of the following:  
+ [agent\_report ](agent-report.md)
+ [ get\_json](agent-json.md)
+ [instance\_report](agent-instance.md)
+ [ list\_commands](agent-list.md)
+ [ run\_command](agent-run.md)
+ [ show\_log ](agent-show.md)
+ [stack\_state ](agent-stack.md)

**activity**  
\(Optional\) Used as an argument with some commands to specify a particular AWS OpsWorks Stacks activity: `setup`, `configure`, `deploy`, `undeploy`, `start`, `stop`, or `restart`\. 

**date**  
\(Optional\) Used as an argument with some commands to specify a particular AWS OpsWorks Stacks command execution\. Specify the command execution by setting **date** to the timestamp that the command was executed in the *yyyy\-mm\-ddThh:mm:ss* format, including the single quotes\. For example, for 10:31:55 on Tuesday Feb 5, 2013, use: `'2013-02-05T10:31:55'`\. To determine when a particular AWS OpsWorks Stacks command was executed, run [ list\_commands](agent-list.md)\.

**Note**  
If the agent has executed the same AWS OpsWorks Stacks activity multiple times, you can pick a particular execution by specifying both the activity and the time it was executed\. If you specify an activity and omit the time, the agent CLI command acts on the activity's most recent execution\. If you omit both arguments, the agent CLI command acts on the most recent activity\.

The following sections describe the commands and their associated arguments\. For brevity, the syntax sections omit the optional `--help` option, which can be used with any command\.

**Topics**
+ [agent\_report](agent-report.md)
+ [get\_json](agent-json.md)
+ [instance\_report](agent-instance.md)
+ [list\_commands](agent-list.md)
+ [run\_command](agent-run.md)
+ [show\_log](agent-show.md)
+ [stack\_state](agent-stack.md)