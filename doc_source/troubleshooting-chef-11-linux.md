# Troubleshooting Chef 11\.10 and Earlier Versions for Linux<a name="troubleshooting-chef-11-linux"></a>

**Note**  
For additional troubleshooting information, see [Debugging and Troubleshooting Guide](troubleshoot.md)\.

## Chef Logs for Chef 11\.10 and Earlier Versions for Linux<a name="troubleshooting-chef-11-linux-logs"></a>

AWS OpsWorks Stacks stores each instance's Chef logs in its `/var/lib/aws/opsworks/chef` directory\. You need sudo privileges to access this directory\. The log for each run is in a file named `YYYY-MM-DD-HH-MM-SS-NN.log`\. 

For more information, see the following:
+ [Viewing a Chef Log with the Console](troubleshoot-debug-log.md#troubleshoot-debug-log-console)
+ [Viewing a Chef Log with the CLI or API](troubleshoot-debug-log.md#troubleshoot-debug-log-cli)
+ [Interpreting a Chef Log](troubleshoot-debug-log.md#troubleshoot-debug-log-interpret)
+ [Common Chef Log Errors](troubleshoot-debug-log.md#troubleshoot-debug-log-errors)