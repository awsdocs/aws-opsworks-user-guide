# Restore an AWS OpsWorks for Chef Automate Server from a Backup<a name="opscm-chef-restore"></a>

After browsing through your available backups, you can choose a point in time from which to restore your AWS OpsWorks for Chef Automate server\. Server backups contain only configuration\-management software persistent data \(cookbooks, registered nodes, etc\.\)\. Performing an in\-place restoration of a server \(that is, restoring the existing AWS OpsWorks for Chef Automate server to a new EC2 instance\) reregisters nodes that were registered at the time of the backup that you use to restore the server, and switches traffic to the new instance if restoration is successful, and the restored AWS OpsWorks for Chef Automate server state is `Healthy`\. Restoring to a newly\-created AWS OpsWorks for Chef Automate server does not maintain node connections\. Restoring a server does not update minor versions of Chef software; it applies the same Chef versions and configuration\-management data that are available in the backup that you choose\.

After restoration is complete, the old EC2 instance remains in a `Running` or `Stopped` state, but only temporarily\. It is eventually terminated\.

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