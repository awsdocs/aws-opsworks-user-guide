# Back Up an OpsWorks for Puppet Enterprise Server<a name="opspup-backup"></a>

You can define a daily or weekly recurring OpsWorks for Puppet Enterprise server backup, and have the service store the backups in Amazon Simple Storage Service \(Amazon S3\) on your behalf\. Alternatively, you can make manual backups on demand\.

Because backups are stored in Amazon S3, they incur additional fees\. You can define a backup retention period of up to 30 generations\. You can submit a service request to have that limit changed by using AWS support channels\. Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

When you configure your OpsWorks for Puppet Enterprise server, you choose either automated or manual backups\. OpsWorks for Puppet Enterprise starts automated backups during the hour and on the day that you choose in the **Automated backup** section of the **Configure advanced settings** page of the setup wizard\. After your server is online, you can change backup settings by performing the following steps on the server's properties page\.

**To change automated backup settings**

1. On the server's properties page, choose **More settings**\.  
![\[Puppet master properties page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_prop_moresettings.png)

1. To turn off automated backups, choose **No** for the **Enable automated backups** option\. Save your changes; you do not need to go on to the next step\.

1. In the **Automated Backup** section, change the frequency, start time, or generations to keep\. Save your changes\.

You can start a manual backup at any time in the AWS Management Console, or by running the AWS CLI [create\-backup](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateBackup.html) command\. Manual backups are not included in the maximum 30 generations of automated backups that are stored; a maximum of 10 manual backups are stored, and must be manually deleted from Amazon S3\.

**To perform a manual backup in the AWS Management Console**

1. On the **Puppet Enterprise servers** page, choose the server that you want to back up\.

1. On the properties page for the server, in the left navigation pane, choose **Backups**\.

1. Choose **Create backup**\.

1. The manual backup is finished when the page shows a green check mark in the backup's **Status** column\.

**To perform a manual backup in the AWS CLI**
+ To start a manual backup, run the following AWS CLI command\.

  ```
  aws opsworks-cm --region region name create-backup --server-name "Puppet server name" --description "optional descriptive string"
  ```

## Delete backups<a name="opscm-puppet-backup-delete"></a>

Deleting a backup permanently deletes it from the S3 bucket in which backups are stored\.

**To delete a backup in the AWS Management Console**

1. On the **Puppet Enterprise servers** page, choose the server that you want to back up\.

1. On the properties page for the server, in the left navigation pane, choose **Backups**\.

1. Choose the backup that you want to delete, and then choose **Delete backup**\. You can select only one backup at a time\.

1. When you are prompted to confirm the deletion, fill the check box for **Delete the backup, which is stored in an S3 bucket**, and then choose **Yes, Delete**\.

**To delete a backup in the AWS CLI**
+ To delete a backup, run the following AWS CLI command, replacing the value of `--backup-id` with the ID of the backup that you want to delete\. Backup IDs are in the format *ServerName\-yyyyMMddHHmmssSSS*\. For example, **puppet\-server\-20171218132604388**\.

  ```
  aws opsworks-cm --region region name delete-backup --backup-id ServerName-yyyyMMddHHmmssSSS
  ```