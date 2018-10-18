# Debugging and Troubleshooting Guide<a name="troubleshoot"></a>

If you need to debug a recipe or troubleshoot a service issue, the best approach generally is to work through the following steps, in order:

1. Check [Common Debugging and Troubleshooting Issues](common-issues.md) for your specific issue\.

1. Search the [AWS OpsWorks Stacks Forum](https://forums.aws.amazon.com/forum.jspa?forumID=153#) to see if the issue has been discussed there\.

   The Forum includes many experienced users and is monitored by the AWS OpsWorks Stacks team\.

1. For issues with recipes, see [Debugging Recipes](troubleshoot-debug.md)\. 

1. Contact AWS OpsWorks Stacks support or post your issue on the [AWS OpsWorks Stacks Forum](https://forums.aws.amazon.com/forum.jspa?forumID=153#)\.

The following section provides guidance for debugging recipes\. The final section describes common debugging and troubleshooting issues and their solutions\.

**Note**  
Each Chef run produces a log, which provides a detailed description of the run and is a valuable troubleshooting resource\. To specify the amount of detail in the log, add a [https://docs.chef.io/resource_log.html](https://docs.chef.io/resource_log.html) statement to a custom recipe that specifies the desired log level\. The default value is `:info`\. The following example shows how to set the Chef log level to `:debug`, which provides the most detailed description of the run\.   

```
Chef::Log.level = :debug
```
For more information about viewing and interpreting Chef logs, see [Chef Logs](troubleshoot-debug-log.md)\.

**Topics**
+ [Debugging Recipes](troubleshoot-debug.md)
+ [Common Debugging and Troubleshooting Issues](common-issues.md)