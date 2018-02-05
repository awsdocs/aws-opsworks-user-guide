# Restore an AWS OpsWorks for Chef Automate Server from a Backup<a name="opscm-chef-restore"></a>

After browsing through your available backups, you can easily choose a point in time from which to restore your AWS OpsWorks for Chef Automate server\. Server backups contain only configuration\-management software persistent data \(cookbooks, registered nodes, etc\.\)\. Performing an in\-place restoration of a server \(that is, restoring to the same EC2 instance\) reregisters nodes that were registered at the time of the backup that you use to restore the server\. Restoring to a new instance does not maintain node connections\. Restoring a server does not update minor versions of Chef software; it applies the same Chef versions and configuration\-management data that are available in the backup that you choose\.

In this release, you can use the AWS CLI to restore a Chef server in AWS OpsWorks for Chef Automate\.

**Note**  
You can also run the [restore\-server](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_RestoreServer.html) command to change the current instance type, or to restore or set your SSH key if it is lost or compromised\.

**To restore a server from a backup**

1. In the AWS CLI, run the following command to return a list of available backups and their IDs\. Make a note of the ID of the backup that you want to use\. Backup IDs are in the format *myServerName\-yyyyMMddHHmmssSSS*\.

   ```
   aws opsworks-cm --region region name describe-backups
   ```

1. Run the following command\.

   ```
   aws opsworks-cm --region region name restore-server --backup-id "myServerName-yyyyMMddHHmmssSSS" --instance-type "Type of instance" --key-pair "name of your EC2 key pair" --server-name "name of Chef server"
   ```

   The following is an example\.

   ```
   aws opsworks-cm --region us-west-2 restore-server --backup-id "MyChefServer-20161120122143125" --server-name "MyChefServer"
   ```

1. Wait until restoration is complete\.