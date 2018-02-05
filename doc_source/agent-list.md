# list\_commands<a name="agent-list"></a>

Lists the times for each activity that has been executed on this instance\. You can use these times for other agent\-CLI commands to specify a particular execution\.

```
sudo opsworks-agent-cli list_commands  [activity] [date]
```

The following output example is from an instance that has run configure, setup, and update custom cookbooks activities\.

```
$ sudo opsworks-agent-cli list_commands

2015-11-24T21:00:28        update_custom_cookbooks
2015-12-01T18:19:09        setup
2015-12-01T18:20:24        configure
```