# Working with Tags on AWS OpsWorks for Puppet Enterprise Resources<a name="opspup-tags"></a>

Tags are words or phrases that act as metadata for identifying and organizing your AWS resources\. In OpsWorks for Puppet Enterprise, a resource can have up to 50 user\-applied tags\. Each tag consists of a key and one optional value\. You can apply tags to the following resources in OpsWorks for Puppet Enterprise:
+ OpsWorks for Puppet Enterprise servers
+ Backups of OpsWorks for Puppet Enterprise servers

Tags on AWS resources can help you track costs, control access to resources, group resources for automating tasks, or organize resources by purpose or lifecycle stage\. For more information about the benefits of tags, see [AWS Tagging Strategies](http://aws.amazon.com/answers/account-management/aws-tagging-strategies/) in AWS Answers and [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.

To use tags to control access to OpsWorks for Puppet Enterprise servers or backups, create or edit policy statements in AWS Identity and Access Management \(IAM\)\. For more information, see [Controlling Access to AWS Resources Using Resource Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *AWS Identity and Access Management User Guide*\.

When you apply tags to an OpsWorks for Puppet Enterprise master, the tags are also applied to the master's backups, the Amazon S3 bucket that stores the backups, the master's Amazon EC2 instance, secrets for the master that are stored in AWS Secrets Manager, and the Elastic IP address used by the master\. Tags are not propagated to the AWS CloudFormation stack that AWS OpsWorks uses to create your Puppet master\.

**Topics**
+ [How Tags Work in AWS OpsWorks for Puppet Enterprise](#opspup-tags-concepts)
+ [Add and Manage Tags in OpsWorks for Puppet Enterprise \(Console\)](#opspup-tags-console)
+ [Add and Manage Tags in OpsWorks for Puppet Enterprise \(CLI\)](#opspup-tags-howto)
+ [See Also](#opspup-tags-seealso)

## How Tags Work in AWS OpsWorks for Puppet Enterprise<a name="opspup-tags-concepts"></a>

In this release, you can add and manage tags by using the [AWS OpsWorks CM API](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/Welcome.html) or the AWS Management Console\. AWS OpsWorks CM also attempts to add tags that you add to a server to the AWS resources that are associated with the server, including the EC2 instance, secrets in Secrets Manager, Elastic IP address, security group, S3 bucket, and backups\.

The following table provides an overview of how you add and manage tags in OpsWorks for Puppet Enterprise\.


| Action | What to use | 
| --- | --- | 
| Add tags to a new OpsWorks for Puppet Enterprise server or a backup that you are creating manually\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opspup-tags.html)  | 
| View tags on a resource\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opspup-tags.html)  | 
| Add tags to an existing OpsWorks for Puppet Enterprise server or a backup, regardless of whether the backup was created manually or automatically\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opspup-tags.html)  | 
| Delete tags from a resource\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opspup-tags.html)  | 

`DescribeServers` and `DescribeBackups` responses do not include tag information\. To show tags, use the `ListTagsForResource` API\.

## Add and Manage Tags in OpsWorks for Puppet Enterprise \(Console\)<a name="opspup-tags-console"></a>

Procedures in this section are performed in the AWS Management Console\.

If you add tags, a tag key cannot be empty\. The key can be a maximum of 127 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @` A tag value is optional\. You can add a tag that has a key, but no value\. The value can be a maximum of 255 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @`

**Topics**
+ [Add Tags to a New OpsWorks for Puppet Enterprise Server \(Console\)](#opspup-tags-new-server-console)
+ [Add Tags to a New Backup \(Console\)](#opspup-tags-new-backup-console)
+ [Add or View Tags on an Existing Server \(Console\)](#opspup-tags-existing-server-console)
+ [Add or View Tags on an Existing Backup \(Console\)](#opspup-tags-existing-backup-console)
+ [Delete Tags from a Server \(Console\)](#opspup-tags-server-delete-console)
+ [Delete Tags from a Backup \(Console\)](#opspup-tags-backup-delete-console)

### Add Tags to a New OpsWorks for Puppet Enterprise Server \(Console\)<a name="opspup-tags-new-server-console"></a>

1. Be sure to complete any [prerequisites](gettingstarted-opspup.md#gettingstarted-opspup-prereqs) for creating an OpsWorks for Puppet Enterprise master\.

1. Follow steps 1\-8 in [Create a Puppet Enterprise Master by using the AWS Management Console](gettingstarted-opspup-create.md#gettingstarted-opspup-create-console)\.

1. After you specify automated backup settings, add tags in the **Tags** area of the **Configure advanced settings** page\. You can add a maximum of 50 tags\. When you are finished adding tags, choose **Next**\.

1. Go on to step 11 of [Create a Puppet Enterprise Master by using the AWS Management Console](gettingstarted-opspup-create.md#gettingstarted-opspup-create-console), and review settings you have chosen for the new server\.

### Add Tags to a New Backup \(Console\)<a name="opspup-tags-new-backup-console"></a>

1. On the OpsWorks for Puppet Enterprise home page, choose an existing Puppet master\.

1. From the server's details page, choose **Backups** in the navigation pane\.

1. On the **Backups** page, choose **Create backup**\.

1. Add tags\. Choose **Create** when you are finished adding tags\.

### Add or View Tags on an Existing Server \(Console\)<a name="opspup-tags-existing-server-console"></a>

1. On the OpsWorks for Puppet Enterprise home page, choose an existing Puppet master to open its details page\.

1. Choose **Tags** in the navigation pane, or at the bottom of the details page, choose **View all tags**\.

1. On the **Tags** page, choose **Edit**\.

1. Add or edit tags on the server\. Choose **Save** when you are finished\.
**Note**  
Be aware that changing tags on your Puppet master also changes tags on resources that are associated with the server, such as the EC2 instance, Elastic IP address, security group, S3 bucket, and backups\.

### Add or View Tags on an Existing Backup \(Console\)<a name="opspup-tags-existing-backup-console"></a>

1. On the OpsWorks for Puppet Enterprise home page, choose an existing Puppet master to open its details page\.

1. Choose **Backups** in the navigation pane, or in the **Recent backups** area of the details page, choose **View all backups**\.

1. On the **Backups** page, choose a backup to manage, and then choose **Edit backup**\.

1. Add or edit tags on the backup\. Choose **Update** when you are finished\.

### Delete Tags from a Server \(Console\)<a name="opspup-tags-server-delete-console"></a>

1. On the OpsWorks for Puppet Enterprise home page, choose an existing Puppet master to open its details page\.

1. Choose **Tags** in the navigation pane, or at the bottom of the details page, choose **View all tags**\.

1. On the **Tags** page, choose **Edit**\.

1. Choose **X** next to a tag to delete the tag\. Choose **Save** when you are finished\.
**Note**  
Be aware that changing tags on your Puppet master also changes tags on resources that are associated with the server, such as the EC2 instance, Elastic IP address, security group, S3 bucket, and backups\.

### Delete Tags from a Backup \(Console\)<a name="opspup-tags-backup-delete-console"></a>

1. On the OpsWorks for Puppet Enterprise home page, choose an existing Puppet master to open its details page\.

1. Choose **Backups** in the navigation pane, or in the **Recent backups** area of the details page, choose **View all backups**\.

1. On the **Backups** page, choose a backup to manage, and then choose **Edit backup**\.

1. Choose **X** next to a tag to delete the tag\. Choose **Update** when you are finished\.

## Add and Manage Tags in OpsWorks for Puppet Enterprise \(CLI\)<a name="opspup-tags-howto"></a>

Procedures in this section are performed in the AWS CLI\. Be sure that you are running the latest release of the AWS CLI before you start working with tags\. For more information about installing or updating the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) in the *AWS Command Line Interface User Guide*\.

If you add tags, a tag key cannot be empty\. The key can be a maximum of 127 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @` A tag value is optional\. You can add a tag that has a key, but no value\. The value can be a maximum of 255 characters, and contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @`

**Topics**
+ [Add Tags to a New OpsWorks for Puppet Enterprise Server \(CLI\)](#opspup-tags-howto-createserver)
+ [Add Tags to a New Backup \(CLI\)](#opspup-tags-howto-createbackup)
+ [Add Tags to Existing Servers or Backups \(CLI\)](#opspup-tags-howto-addtags)
+ [List Resource Tags \(CLI\)](#opspup-tags-howto-listtags)
+ [Delete Tags from a Resource \(CLI\)](#opspup-tags-howto-untag)

### Add Tags to a New OpsWorks for Puppet Enterprise Server \(CLI\)<a name="opspup-tags-howto-createserver"></a>

You can use the AWS CLI to add tags when you create an OpsWorks for Puppet Enterprise server\. This procedure does not describe in full how to create a server\. For detailed information about how to create an OpsWorks for Puppet Enterprise server by using the AWS CLI, see [Create a Puppet Enterprise Master by using the AWS CLI](gettingstarted-opspup-create.md#gettingstarted-opspup-create-cli) in this guide\. You can add up to 50 tags to a server\.

1. Be sure to complete any [prerequisites](gettingstarted-opspup.md#gettingstarted-opspup-prereqs) for creating an OpsWorks for Puppet Enterprise server\.

1. Complete steps 1\-4 of [Create a Puppet Enterprise Master by using the AWS CLI](gettingstarted-opspup-create.md#gettingstarted-opspup-create-cli)\.

1. For step 5, when you run the `create-server` command, add the `--tags` parameter to the command, as shown in the following example\.

   ```
   aws opsworks-cm create-server ... --tags Key=Key1,Value=Value1 Key=Key2,Value=Value2
   ```

   The following is an example showing only the tags portion of the `create-server` command\.

   ```
   aws opsworks-cm create-server ... --tags Key=Stage,Value=Production Key=Department,Value=Marketing
   ```

1. Complete the remaining steps in [Create a Puppet Enterprise Master by using the AWS CLI](gettingstarted-opspup-create.md#gettingstarted-opspup-create-cli)\. To verify that your tags were added to the new server, follow steps in [List Resource Tags \(CLI\)](#opspup-tags-howto-listtags) in this topic\.

### Add Tags to a New Backup \(CLI\)<a name="opspup-tags-howto-createbackup"></a>

You can use the AWS CLI to add tags when you create a new, manual backup of an OpsWorks for Puppet Enterprise server\. This procedure does not describe in full how to create a manual backup\. For detailed information about how to create a manual backup, see "To perform a manual backup in the AWS CLI" in [Back Up an OpsWorks for Puppet Enterprise Server](opspup-backup.md)\. You can add up to 50 tags to a backup\. If a server has tags, new backups are automatically tagged with the server's tags\.

By default, when you create a new OpsWorks for Puppet Enterprise server, automated backups are enabled\. You can add tags to an automated backup by running the `tag-resource` command, described in [Add Tags to Existing Servers or Backups \(CLI\)](#opspup-tags-howto-addtags) in this topic\.
+ To add tags to a manual backup as you're creating the backup, run the following command\. Only the tags portion of the command is shown\. For an example of the full `create-backup` command, see "To perform a manual backup in the AWS CLI" in [Back Up an OpsWorks for Puppet Enterprise Server](opspup-backup.md)\.

  ```
  aws opsworks-cm create-backup ... --tags Key=Key1,Value=Value1 Key=Key2,Value=Value2
  ```

  The following example shows only the tags portion of the `create-backup` command\.

  ```
  aws opsworks-cm create-backup ... --tags Key=Stage,Value=Production Key=Department,Value=Marketing
  ```

### Add Tags to Existing Servers or Backups \(CLI\)<a name="opspup-tags-howto-addtags"></a>

You can run the `tag-resource` command to add tags to existing OpsWorks for Puppet Enterprise servers or backups \(whether the backups were created automatically or manually\)\. Specify the Amazon Resource Number \(ARN\) of a target resource to add tags to it\.

1. To get the ARN of the resource to which you want to apply tags:
   + For a server, run `describe-servers --server-name server_name`\. The results of the command show the server ARN\.
   + For a backup, run `describe-backups --backup-id backup_ID`\. The results of the command show the backup ARN\. You can also run `describe-backups --server-name server_name` to show information about all backups for a specific OpsWorks for Puppet Enterprise server\.

   The following example shows only the `ServerArn` in results of a `describe-servers --server-name opsworks-cm-test` command\. The `ServerArn` value is added to a `tag-resource` command to add tags to the server\.

   ```
   {
       "Servers": [
           {
               ...
               "ServerArn": "arn:aws:opsworks-cm:us-west-2:123456789012:server/opsworks-cm-test/EXAMPLEd-66b0-4196-8274-d1a2bEXAMPLE"
           }
       ]
   }
   ```

1. Run the `tag-resource` command with the ARN that you returned in step 1\.

   ```
   aws opsworks-cm tag-resource --resource-arn "server_or_backup_ARN" --tags Key=Key1,Value=Value1 Key=Key2,Value=Value2
   ```

   The following is an example\.

   ```
   aws opsworks-cm tag-resource --resource-arn "arn:aws:opsworks-cm:us-west-2:123456789012:server/opsworks-cm-test/EXAMPLEd-66b0-4196-8274-d1a2bEXAMPLE" --tags Key=Stage,Value=Production Key=Department,Value=Marketing
   ```

1. To verify that tags were added successfully, go on to the next procedure, [List Resource Tags \(CLI\)](#opspup-tags-howto-listtags)\.

### List Resource Tags \(CLI\)<a name="opspup-tags-howto-listtags"></a>

You can run the `list-tags-for-resource` command to show the tags that are attached to OpsWorks for Puppet Enterprise servers or backups\. Specify the ARN of a target resource to view its tags\.

1. To get the ARN of the resource for which you want to list tags:
   + For a server, run `describe-servers --server-name server_name`\. The results of the command show the server ARN\.
   + For a backup, run `describe-backups --backup-id backup_ID`\. The results of the command show the backup ARN\. You can also run `describe-backups --server-name server_name` to show information about all backups for a specific OpsWorks for Puppet Enterprise server\.

1. Run the `list-tags-for-resource` command with the ARN that you returned in step 1\.

   ```
   aws opsworks-cm list-tags-for-resource --resource-arn "server_or_backup_ARN"
   ```

   The following is an example\.

   ```
   aws opsworks-cm tag-resource --resource-arn "arn:aws:opsworks-cm:us-west-2:123456789012:server/opsworks-cm-test/EXAMPLEd-66b0-4196-8274-d1a2bEXAMPLE"
   ```

   If there are tags on the resource, the command returns results like the following\.

   ```
   {
       "Tags": [
           {
               "Key": "Stage",
               "Value": "Production"
           },
           {
               "Key": "Department",
               "Value": "Marketing"
           }
       ]
   }
   ```

### Delete Tags from a Resource \(CLI\)<a name="opspup-tags-howto-untag"></a>

You can run the `untag-resource` command to delete tags from OpsWorks for Puppet Enterprise servers or backups\. If the resource is deleted, tags on the resource are also deleted\. Specify the Amazon Resource Number \(ARN\) of a target resource to remove tags from it\.

1. To get the ARN of the resource from which you want to remove tags:
   + For a server, run `describe-servers --server-name server_name`\. The results of the command show the server ARN\.
   + For a backup, run `describe-backups --backup-id backup_ID`\. The results of the command show the backup ARN\. You can also run `describe-backups --server-name server_name` to show information about all backups for a specific OpsWorks for Puppet Enterprise server\.

1. Run the `untag-resource` command with the ARN that you returned in step 1\. Specify only the tags that you want to delete\.

   ```
   aws opsworks-cm untag-resource --resource-arn "server_or_backup_ARN" --tags Key=Key1,Value=Value1 Key=Key2,Value=Value2
   ```

   In this example, the `untag-resource` command removes only the tag with a key of `Stage` and a value of `Production`\.

   ```
   aws opsworks-cm untag-resource --resource-arn "arn:aws:opsworks-cm:us-west-2:123456789012:server/opsworks-cm-test/EXAMPLEd-66b0-4196-8274-d1a2bEXAMPLE" --tags Key=Stage,Value=Production
   ```

1. To verify that tags were deleted successfully, follow steps in [List Resource Tags \(CLI\)](#opspup-tags-howto-listtags) in this topic\.

## See Also<a name="opspup-tags-seealso"></a>
+ [Create a Puppet Enterprise Master by using the AWS CLI](gettingstarted-opspup-create.md#gettingstarted-opspup-create-cli)
+ [Back Up an OpsWorks for Puppet Enterprise Server](opspup-backup.md)
+ [AWS Tagging Strategies](http://aws.amazon.com/answers/account-management/aws-tagging-strategies/)
+ [Controlling Access to AWS Resources Using Resource Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *AWS Identity and Access Management User Guide*
+ [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*
+ [CreateBackup](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateBackup.html) in the *AWS OpsWorks CM API Reference*
+ [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) in the *AWS OpsWorks CM API Reference*
+ [TagResource](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_TagResource.html) in the *AWS OpsWorks CM API Reference*
+ [ListTagsForResource](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_ListTagsForResource.html) in the *AWS OpsWorks CM API Reference*
+ [UntagResource](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_UntagResource.html) in the *AWS OpsWorks CM API Reference*