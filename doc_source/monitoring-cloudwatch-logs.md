# Using Amazon CloudWatch Logs with AWS OpsWorks Stacks<a name="monitoring-cloudwatch-logs"></a>

To simplify the process of monitoring logs on multiple instances, AWS OpsWorks Stacks supports Amazon CloudWatch Logs\. You enable CloudWatch Logs at the layer level in AWS OpsWorks Stacks\. CloudWatch Logs integration works with Chef 11\.10 and Chef 12 Linux\-based stacks\. You incur additional charges when you enable CloudWatch Logs, so review [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/) before you get started\.

CloudWatch Logs monitors selected logs for the occurrence of a user\-specified pattern\. For example, you can monitor logs for the occurrence of a literal term such as `NullReferenceException`, or count the number of such occurrences\. After you enable CloudWatch Logs in AWS OpsWorks Stacks, the AWS OpsWorks Stacks agent sends the logs to CloudWatch Logs\. For more information about CloudWatch Logs, see [Getting Started with CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_GettingStarted.html)\.

## Prerequisites<a name="w4ab1c11c57c13b7"></a>

Before you can enable CloudWatch Logs, your instances must be running version 3444 or later of the AWS OpsWorks Stacks agent in Chef 11\.10 stacks, and 4023 or later in Chef 12 stacks\. You must also use a compatible instance profile for any instances that you are monitoring by using CloudWatch Logs\. AWS OpsWorks Stacks prompts you to let it upgrade the agent version and instance profile when you open the CloudWatch Logs tab on the **Layer** page\.

If you are using a custom instance profile \(one that AWS OpsWorks Stacks did not provide when you created the stack\), AWS OpsWorks Stacks cannot automatically upgrade the instance profile\. You must manually attach the **AWSOpsWorksCloudWatchLogs** policy to your profile by using IAM\. For information, see [Attaching Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console) in the *IAM User Guide*\.

The following screenshot shows the upgrade prompt\.

![\[CloudWatch Logs tab on the Layer page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cw_logs_upgrade.png)

Updating the agent on all instances in a layer can take some time\. If you try to enable CloudWatch Logs on a layer before the agent upgrade is complete, you see a message similar to the following\.

![\[CloudWatch Logs tab on the Layer page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cloudwatch_logs_upgrade_time.png)

## Enabling CloudWatch Logs<a name="w4ab1c11c57c13b9"></a>

1. After any required agent and instance profile upgrades are complete, you can enable CloudWatch Logs by setting the slider control on the **CloudWatch Logs** tab to **On**\.  
![\[CloudWatch Logs slider control\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cw_logs_enable_switch.png)

1. To stream command logs, set the **Stream command logs** slider to **On**\. This sends logs of Chef activities and user\-initiated commands on your layer's instances to CloudWatch Logs\.

   The data included in these logs closely matches what you see in the results of a [DescribeCommands](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeCommands.html) operation, when you open the target of the log URL\. It includes data about `setup`, `configure`, `deploy`, `undeploy`, `start`, `stop`, and recipe run commands\.

1. To stream logs of activities that are stored in a custom location on your layer's instances, such as `/var/log/apache/myapp/mylog*`, type the custom location in the **Stream custom logs** string box, and then choose **Add** \(**\+**\)\.

1. Choose **Save**\. Within a few minutes, AWS OpsWorks Stacks log streams should be visible in the CloudWatch Logs console\.  
![\[CloudWatch Logs is enabled\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cw_logs_enabled.png)

## Turning Off CloudWatch Logs<a name="w4ab1c11c57c13c11"></a>

To turn off CloudWatch Logs, edit your layer settings\.

1. On your layer's properties page, choose **Edit**\.  
![\[Edit button on Layer properties page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cw_logs_enabled_edit.png)

1. On the editing page, choose the **CloudWatch Logs** tab\.

1. In the **CloudWatch Logs** area, turn off **Stream command logs**\. Choose **X** on custom logs to delete them from log streams, if applicable\.

1. Choose **Save**\.

### Deleting Streamed Logs from CloudWatch Logs<a name="w4ab1c11c57c13c11b7"></a>

After you turn off CloudWatch Logs streaming from AWS OpsWorks Stacks, existing logs are still available in the CloudWatch Logs management console\. You still incur charges for stored logs, unless you export the logs to Amazon S3 or delete them\. For more information about exporting logs to S3, see [Exporting Log Data to Amazon S3](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/S3Export.html)\.

You can delete log streams and log groups in the CloudWatch Logs management console, or by running the [http://docs.aws.amazon.com/cli/latest/reference/logs/delete-log-stream.html](http://docs.aws.amazon.com/cli/latest/reference/logs/delete-log-stream.html) and [http://docs.aws.amazon.com/cli/latest/reference/logs/delete-log-group.html](http://docs.aws.amazon.com/cli/latest/reference/logs/delete-log-group.html) AWS CLI commands\. For more information about changing log retention periods, see [Change Log Data Retention in CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SettingLogRetention.html)\.

## Managing Your Logs in CloudWatch Logs<a name="w4ab1c11c57c13c13"></a>

The logs that you are streaming are managed in the CloudWatch Logs console\.

![\[CloudWatch Logs console\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cw_logs_dash.png)

AWS OpsWorks creates default log groups and log streams automatically\. Log groups for AWS OpsWorks Stacks data have names that match the following pattern:

*stack\_name*`/`*layer\_name*`/`*chef\_log\_name*

Custom logs have names that match the following pattern:

*/stack\_name/layer\_short\_name/file\_path\_name*\. The path name is made more human\-readable by the removal of special characters, such as asterisks \(\*\)\.

When you've located your logs in CloudWatch Logs, you can [organize the logs into groups](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Create-Log-Group.html), [search and filter logs by creating metric filters](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html), and [create custom alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html)\.

## Configuring Chef 12\.2 Windows Layers to Use CloudWatch Logs<a name="w4ab1c11c57c13c15"></a>

CloudWatch Logs automatic integration is not supported for Windows\-based instances\. The **CloudWatch Logs** tab is not available on layers in Chef 12\.2 stacks\. To manually enable streaming to CloudWatch Logs for Windows\-based instances, do the following\.
+ Update the instance profile for Windows\-based instances so that the CloudWatch Logs agent has appropriate permissions\. The **AWSOpsWorksCloudWatchLogs** policy statement shows which permissions are required\.

  Typically, you do this task only once\. You can then use the updated instance profile for all Windows instances in a layer\.
+ Edit the following JSON configuration file on each instance\. This file includes log stream preferences, such as which logs to monitor\.

  `%PROGRAMFILES%\Amazon\Ec2ConfigService\Settings\AWS.EC2.Windows.CloudWatch.json`

You can automate the preceding two tasks by creating custom recipes to handle the required tasks and assigning them to the Chef 12\.2 layer's **Setup** events\. Each time you start a new instance on those layers, AWS OpsWorks Stacks automatically runs your recipes after the instance finishes booting, enabling CloudWatch Logs\. For more information about manually configuring CloudWatch Logs streams for Windows\-based instances, see the following\.
+ [Configuring a Windows Instance Using the EC2Config Service](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html#send_logs_to_cwl)
+ [Sending Logs, Events, and Performance Counters to Amazon CloudWatch](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/send_logs_to_cwl.html)
+ [CloudWatch Update – Enhanced Support for Windows Log Files](https://aws.amazon.com/blogs/aws/additional-cloudwatch-logs-windows/) \(blog post\)

To turn off CloudWatch Logs on Windows\-based instances, reverse the process\. Clear the **Enable CloudWatch Logs integration** check box in the **EC2 Service Properties** dialog box, delete log stream preferences from the `AWS.EC2.Windows.CloudWatch.json` file; and stop running any Chef recipes that are automatically assigning CloudWatch Logs permissions to new instances in Chef 12\.2 layers\.