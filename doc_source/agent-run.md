# run\_command<a name="agent-run"></a>

Runs an AWS OpsWorks Stacks command, which is a JSON file containing a Chef run\-list that contains the information necessary to execute an AWS OpsWorks Stacks activity \(setup, configure, deploy, and so on\)\. The `run_command` command generates a log entry that you can view by running [ show\_log ](agent-show.md)\. This option is intended only for development purposes, so AWS OpsWorks Stacks does not track changes\. 

```
sudo opsworks-agent-cli run_command [activity] [date] [/path/to/valid/json.file]
```

 By default, `run_command` runs the most recent AWS OpsWorks Stacks command\. Use the following options to specify a particular command\.

**activity**  
Run a specified AWS OpsWorks Stacks command: `setup`, `configure`, `deploy`, `undeploy`, `start`, `stop`, or `restart`\.

**date**  
Run the AWS OpsWorks command that executed at the specified timestamp\. To get a list of valid timestamps, run [ list\_commands](agent-list.md)\.

**file**  
Run the specified command JSON file\. To get a command's file path, run [ get\_json](agent-json.md)\.

The following output example is from an instance and runs the configure command\.

```
$ sudo opsworks-agent-cli run_command configure

[2015-12-02 16:52:53]  INFO [opsworks-agent(21970)]: About to re-run 'configure' from 2015-12-01T18:20:24
...
[2015-12-02 16:53:02]  INFO [opsworks-agent(21970)]: Finished Chef run with exitcode 0
```