# Upgrade an AWS OpsWorks for Chef Automate Server to Chef Automate 2<a name="opscm-a2upgrade"></a>

## Prerequisites for Upgrading to Chef Automate 2<a name="opscm-a2upgrade-prereqs"></a>

Before you get started, be sure you understand the new features that Chef Automate 2 adds, and features that Chef Automate 2 does not support\. For information about the new and unsupported features in Chef Automate 2, see the [Chef Automate 2 documentation](https://automate.chef.io/docs/upgrade/#considerations) on the Chef website\.

A server running Chef Automate 1 must have had at least one successful maintenance run after November 1, 2019 to be eligible for upgrade\.

As with any maintenance operation on your AWS OpsWorks for Chef Automate server, the server is offline during the upgrade\. You should plan for up to three hours of downtime during the upgrade process\.

You need the sign\-in credentials for this server for the Chef Automate dashboard website\. When the upgrade is finished, you should sign in to the Chef Automate dashboard and verify that your nodes and configuration information are not changed\.

**Important**  
When you are ready to upgrade your AWS OpsWorks for Chef Automate server to Chef Automate 2, use only the instructions here to upgrade\. Because AWS OpsWorks for Chef Automate automates many of the upgrade processes, such as backup creation, do not follow upgrade instructions on the Chef website\.

## About the Upgrade Process<a name="opscm-a2upgrade-whathappens"></a>

During the upgrade process, your server is backed up before starting upgrade and after finishing the upgrade\. The following backups are created: 
+ A backup of the server when it's still running Chef Automate 1 \(version 12\.17\.33\)\.
+ A backup of the server after upgrade is finished and the server is running Chef Automate 2 \(version 2019\-08\)\.

The upgrade process terminates the Amazon EC2 instance that the server was using when it ran Chef Automate 1\. A new instance is created to run the Chef Automate 2 server\.

## Upgrade to Chef Automate 2 \(Console\)<a name="opscm-a2upgrade-console"></a>

1. Sign in to the AWS Management Console and open the AWS OpsWorks console at [https://console\.aws\.amazon\.com/opsworks/](https://console.aws.amazon.com/opsworks/)\.

1. In the left navigation pane, choose **AWS OpsWorks for Chef Automate**\.

1. Choose a server to view its properties page\. A blue banner at the top of the page should indicate whether the server is eligible for upgrade to Chef Automate 2\.
**Note**  
A server running Chef Automate 1 must have had at least one successful maintenance run after November 1, 2019 to be eligible for upgrade\.

1. If the server is eligible for upgrade, choose **Start upgrade**\.

1. Allow up to three hours for upgrade\. During the upgrade process, the properties page displays server status as **Under maintenance**\.

1. When the upgrade is finished, the properties page displays the following two messages: **Successfully upgraded to Automate 2** and **Maintenance completed successfully**\. The server status should be **HEALTHY**\.

1. Sign in to the Chef Automate dashboard with your existing credentials, and verify that your nodes are reporting correctly\.

## Upgrade to Chef Automate 2 \(CLI\)<a name="opscm-a2upgrade-cli"></a>

1. \(Optional\) If you aren't sure which of your AWS OpsWorks for Chef Automate servers are eligible for upgrade, run the following command\. Be sure to add the `--region` parameter if you want to list AWS OpsWorks for Chef Automate servers in an AWS Region that is different from your default AWS Region\.

   ```
   aws opsworks-cm describe-servers
   ```

   In the results, look for the a value of `true` for the attribute `CHEF_MAJOR_UPGRADE_AVAILABLE`\. This indicates that the server is eligible for upgrade to Chef Automate 2\. Make a note of the names of AWS OpsWorks for Chef Automate servers that are eligible for upgrade\.

1. Run the following command, replacing *server\_name* with the name of an AWS OpsWorks for Chef Automate server\. To upgrade to Chef Automate 2 instead of performing routine system maintenance, add the `CHEF_MAJOR_UPGRADE` engine attribute, as shown in the command\. Add the `--region` parameter if the target server is not in your default AWS Region\. You can only upgrade one server per command\.

   ```
   aws opsworks-cm start-maintenance --server-name server_name --engine-attributes Name=CHEF_MAJOR_UPGRADE,Value=true --region region
   ```

   If AWS OpsWorks for Chef Automate cannot upgrade the server for any reason, this command results in a validation exception\.

1. Allow up to three hours for the upgrade\. You can check the upgrade status periodically by running the following command\.

   ```
   aws opsworks-cm describe-servers --server-name server_name
   ```

   In the results, look for the `Status` value\. A `Status` of `UNDER_MAINTENANCE` means that the upgrade is still in progress\. A successful upgrade returns messages similar to the following\.

   ```
   2019/10/24 00:27:56 UTC           Successfully upgraded to Automate 2.
   2019/10/23 23:50:38 UTC           Upgrading Chef server from Automate 1 to Automate 2
   ```

   If the upgrade was unsuccessful, AWS OpsWorks for Chef Automate automatically rolls back your server to Chef Automate 1\.

   If the upgrade was successful but the server is not functioning the same as before the upgrade \(for example, if managed nodes are not reporting\), you can roll the server back manually\. For manual rollback information, see \.

## Roll Back an AWS OpsWorks for Chef Automate Server to Chef Automate 1 \(CLI\)<a name="opscm-a2upgrade-rollback-cli"></a>

If the upgrade process fails, AWS OpsWorks for Chef Automate automatically rolls your server back to Chef Automate 1\. If the upgrade was successful but the server is not functioning the same as before the upgrade, you can roll your AWS OpsWorks for Chef Automate server back to Chef Automate 1 manually by using the AWS CLI\.

1. Run the following command to show the `BackupId` of the last backup performed on your server before you attempted the upgrade\. Add the `--region` parameter if your server is in an AWS Region that is different from your default AWS Region\.

   ```
   aws opsworks-cm describe-backups server_name
   ```

   Backup IDs are in the format *ServerName\-yyyyMMddHHmmssSSS*\. Look for the following Chef Automate 1 properties in the results\.

   ```
   "Engine": "Chef"
   "EngineVersion": "12.17.33"
   ```

1. Run the following command, using the backup ID you returned in step 1 as the value of `--backup-id`\.

   ```
   aws opsworks-cm restore-server --server-name server_name --backup-id ServerName-yyyyMMddHHmmssSSS
   ```

   Allow between 20 minutes and three hours to restore the server, depending on the amount of data you stored on the server\. During the restore operation, your server has a status of `RESTORING`\. This status is displayed on the server's properties page in the AWS Management Console, and returned in the results of the describe\-servers command\.

1. After restoration is finished, the console displays the message, **Restore completed successfully**\. Your AWS OpsWorks for Chef Automate server is online, and the same as it was before you started the upgrade process\.

## See Also<a name="opscm-a2upgrade-seealso"></a>
+ [System Maintenance in AWS OpsWorks for Chef Automate](opscm-maintenance.md)
+ [Restore an AWS OpsWorks for Chef Automate Server from a Backup](opscm-chef-restore.md)
+ [https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DescribeServers.html](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DescribeServers.html) in the *AWS OpsWorks API Reference*
+ [https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_StartMaintenance.html](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_StartMaintenance.html) in the *AWS OpsWorks API Reference*