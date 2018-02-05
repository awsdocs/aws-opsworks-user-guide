# show\_log<a name="agent-show"></a>

Return's a command's log file\.

```
sudo opsworks-agent-cli show_log [activity] [date]
```

 By default, `show_log` tails the most recent log file\. Use the following options to specify a particular command\.

**activity**  
Display the specified activity's log file\.

**date**  
Display the log file for the activity that executed at the specified timestamp\. To get a list of valid timestamps, run [ list\_commands](agent-list.md)\.

The following output example shows the most recent log\.

```
$ sudo opsworks-agent-cli show_log

[2015-12-02T16:52:59+00:00] INFO: Storing updated cookbooks/opsworks_cookbook_demo/opsworks-cookbook-demo.tar.gz in the cache.
...
[2015-12-02T16:52:59+00:00] INFO: Report handlers complete
```