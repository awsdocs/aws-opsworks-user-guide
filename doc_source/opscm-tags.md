# Working with Tags on AWS OpsWorks for Chef Automate Resources<a name="opscm-tags"></a>

Tags are words or phrases that act as metadata for identifying and organizing your AWS resources\. In AWS OpsWorks for Chef Automate, a resource can have up to 50 user\-applied tags\. Each tag consists of a key and one optional value\. You can apply tags to the following resources in AWS OpsWorks for Chef Automate:
+ AWS OpsWorks for Chef Automate servers
+ Backups of AWS OpsWorks for Chef Automate servers

Tags on AWS resources can help you track costs, control access to resources, group resources for automating tasks, or organize resources by purpose or lifecycle stage\. For more information about the benefits of tags, see [AWS Tagging Strategies](http://aws.amazon.com/answers/account-management/aws-tagging-strategies/) in AWS Answers and [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.

To use tags to control access to AWS OpsWorks for Chef Automate servers or backups, you create or edit policy statements in AWS Identity and Access Management \(IAM\)\. For more information, see [Controlling Access to AWS Resources Using Resource Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *AWS Identity and Access Management User Guide*\.

When you apply tags to an AWS OpsWorks for Chef Automate server, the tags are also applied to the server's backups, the Amazon S3 bucket that stores the backups, the server's Amazon EC2 instance, secrets for the server that are stored in AWS Secrets Manager, and the Elastic IP address used by the server\. Tags are not propagated to the AWS CloudFormation stack that AWS OpsWorks uses to create your server\.

**Topics**
+ [How Tags Work in AWS OpsWorks for Chef Automate](#opscm-tags-concepts)
+ [Add and Manage Tags in AWS OpsWorks for Chef Automate \(Console\)](#opscm-tags-howto-console)
+ [Add and Manage Tags in AWS OpsWorks for Chef Automate \(CLI\)](#opscm-tags-howto)
+ [See Also](#opscm-tags-seealso)

## How Tags Work in AWS OpsWorks for Chef Automate<a name="opscm-tags-concepts"></a>

In this release, you can add and manage tags by using the [AWS OpsWorks CM API](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/Welcome.html) or the AWS Management Console\. AWS OpsWorks CM also attempts to add tags that you add to a server to the AWS resources that are associated with the server, including the EC2 instance, secrets in Secrets Manager, Elastic IP address, security group, S3 bucket, and backups\. The following table provides an overview of how you add and manage tags in AWS OpsWorks for Chef Automate\.


| Action | What to use | 
| --- | --- | 
| Add tags to a new AWS OpsWorks for Chef Automate server or a backup that you are creating manually\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opscm-tags.html)  | 
| View tags on a resource\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opscm-tags.html)  | 
| Add tags to an existing AWS OpsWorks for Chef Automate server or a backup, regardless of whether the backup was created manually or automatically\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opscm-tags.html)  | 
| Delete tags from a resource\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opscm-tags.html)  | 

`DescribeServers` and `DescribeBackups` responses do not include tag information\. To show tags, use the `ListTagsForResource` API\.

## Add and Manage Tags in AWS OpsWorks for Chef Automate \(Console\)<a name="opscm-tags-howto-console"></a>

Procedures in this section are performed in the AWS Management Console\.

If you add tags, a tag key cannot be empty\. The key can be a maximum of 127 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @` A tag value is optional\. You can add a tag that has a key, but no value\. The value can be a maximum of 255 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @`

**Topics**
+ [Add Tags to a New AWS OpsWorks for Chef Automate Server \(Console\)](#opscm-tags-howto-createserver-console)
+ [Add Tags to a New Backup \(Console\)](#opscm-tags-howto-createbackup-console)
+ [Add or View Tags on an Existing Server \(Console\)](#opscm-tags-howto-server-tag-console)
+ [Add or View Tags on an Existing Backup \(Console\)](#opscm-tags-existing-backup-console)
+ [Delete Tags from a Server \(Console\)](#opscm-tags-delete-server-console)
+ [Delete Tags from a Backup \(Console\)](#opscm-tags-delete-backup-console)

### Add Tags to a New AWS OpsWorks for Chef Automate Server \(Console\)<a name="opscm-tags-howto-createserver-console"></a>

1. Be sure to complete any [prerequisites](gettingstarted-opscm.md#gettingstarted-opscm-prereq-customdomain) for creating an AWS OpsWorks for Chef Automate server\.

1. Follow steps 1\-10 in [Create a Chef Automate Server](gettingstarted-opscm-create.md)\.

1. After you specify automated backup settings, add tags in the **Tags** area of the **Configure advanced settings** page\. You can add a maximum of 50 tags\. When you are finished adding tags, choose **Next**\.

1. Go on to step 13 of [Create a Chef Automate Server](gettingstarted-opscm-create.md), and review settings you have chosen for the new server\.

### Add Tags to a New Backup \(Console\)<a name="opscm-tags-howto-createbackup-console"></a>

1. On the AWS OpsWorks for Chef Automate home page, choose an existing Chef Automate server\.

1. From the server's details page, choose **Backups** in the navigation pane\.

1. On the **Backups** page, choose **Create backup**\.

1. Add tags\. Choose **Create** when you are finished adding tags\.

### Add or View Tags on an Existing Server \(Console\)<a name="opscm-tags-howto-server-tag-console"></a>

1. On the AWS OpsWorks for Chef Automate home page, choose an existing Chef Automate server to open its details page\.

1. Choose **Tags** in the navigation pane, or at the bottom of the details page, choose **View all tags**\.

1. On the **Tags** page, choose **Edit**\.

1. Add or edit tags on the server\. Choose **Save** when you are finished\.
**Note**  
Be aware that changing tags on your Chef Automate server also changes tags on resources that are associated with the server, such as the EC2 instance, Elastic IP address, security group, S3 bucket, and backups\.

### Add or View Tags on an Existing Backup \(Console\)<a name="opscm-tags-existing-backup-console"></a>

1. On the AWS OpsWorks for Chef Automate home page, choose an existing Chef Automate server to open its details page\.

1. Choose **Backups** in the navigation pane, or in the **Recent backups** area of the details page, choose **View all backups**\.

1. On the **Backups** page, choose a backup to manage, and then choose **Edit backup**\.

1. Add or edit tags on the backup\. Choose **Update** when you are finished\.

### Delete Tags from a Server \(Console\)<a name="opscm-tags-delete-server-console"></a>

1. On the AWS OpsWorks for Chef Automate home page, choose an existing Chef Automate server to open its details page\.

1. Choose **Tags** in the navigation pane, or at the bottom of the details page, choose **View all tags**\.

1. On the **Tags** page, choose **Edit**\.

1. Choose **X** next to a tag to delete the tag\. Choose **Save** when you are finished\.
**Note**  
Be aware that changing tags on your Chef Automate server also changes tags on resources that are associated with the server, such as the EC2 instance, Elastic IP address, security group, S3 bucket, and backups\.

### Delete Tags from a Backup \(Console\)<a name="opscm-tags-delete-backup-console"></a>

1. On the AWS OpsWorks for Chef Automate home page, choose an existing Chef Automate server to open its details page\.

1. Choose **Backups** in the navigation pane, or in the **Recent backups** area of the details page, choose **View all backups**\.

1. On the **Backups** page, choose a backup to manage, and then choose **Edit backup**\.

1. Choose **X** next to a tag to delete the tag\. Choose **Update** when you are finished\.

## Add and Manage Tags in AWS OpsWorks for Chef Automate \(CLI\)<a name="opscm-tags-howto"></a>

Procedures in this section are performed in the AWS CLI\. Be sure that you are running the latest release of the AWS CLI before you start working with tags\. For more information about installing or updating the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) in the *AWS Command Line Interface User Guide*\.

If you add tags, a tag key cannot be empty\. The key can be a maximum of 127 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @` A tag value is optional\. You can add a tag that has a key, but no value\. The value can be a maximum of 255 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / @`

**Topics**
+ [Add Tags to a New AWS OpsWorks for Chef Automate Server \(CLI\)](#opscm-tags-howto-createserver)
+ [Add Tags to a New Backup \(CLI\)](#opscm-tags-howto-createbackup)
+ [Add Tags to Existing Servers or Backups \(CLI\)](#opscm-tags-howto-addtags)
+ [List Resource Tags](#opscm-tags-howto-listtags)
+ [Delete Tags from a Resource](#opscm-tags-howto-untag)

### Add Tags to a New AWS OpsWorks for Chef Automate Server \(CLI\)<a name="opscm-tags-howto-createserver"></a>

You can use the AWS CLI to add tags when you create an AWS OpsWorks for Chef Automate server\. This procedure does not describe in full how to create a server\. For detailed information about how to create an AWS OpsWorks for Chef Automate server by using the AWS CLI see [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli) in this guide\. You can add up to 50 tags to a server\.

1. Be sure to complete any [prerequisites](gettingstarted-opscm.md#gettingstarted-opscm-prereq-customdomain) for creating an AWS OpsWorks for Chef Automate server\.

1. Complete steps 1\-5 of [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli)\.

1. For step 6, when you run the `create-server` command, add the `--tags` parameter to the command, as shown in the following example\.

   ```
   aws opsworks-cm create-server ... --tags Key=Key1,Value=Value1 Key=Key2,Value=Value2
   ```

   The following is an example showing only the tags portion of the `create-server` command\.

   ```
   aws opsworks-cm create-server ... --tags Key=Stage,Value=Production Key=Department,Value=Marketing
   ```

1. Complete the remaining steps in [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli)\. To verify that your tags were added to the new server, follow steps in [List Resource Tags](#opscm-tags-howto-listtags) in this topic\.

### Add Tags to a New Backup \(CLI\)<a name="opscm-tags-howto-createbackup"></a>

You can use the AWS CLI to add tags when you create a new, manual backup of an AWS OpsWorks for Chef Automate server\. This procedure does not describe in full how to create a manual backup\. For detailed information about how to create a manual backup, see "To perform a manual backup in the AWS CLI" in [Back Up an AWS OpsWorks for Chef Automate Server](opscm-chef-backup.md)\. You can add up to 50 tags to a backup\. If a server has tags, new backups are automatically tagged with the server's tags\.

By default, when you create a new AWS OpsWorks for Chef Automate server, automated backups are enabled\. You can add tags to an automated backup by running the `tag-resource` command, described in [Add Tags to Existing Servers or Backups \(CLI\)](#opscm-tags-howto-addtags) in this topic\.
+ To add tags to a manual backup as you're creating the backup, run the following command\. Only the tags portion of the command is shown\. For an example of the full `create-backup` command, see "To perform a manual backup in the AWS CLI" in [Back Up an AWS OpsWorks for Chef Automate Server](opscm-chef-backup.md)\.

  ```
  aws opsworks-cm create-backup ... --tags Key=Key1,Value=Value1 Key=Key2,Value=Value2
  ```

  The following example shows only the tags portion of the `create-backup` command\.

  ```
  aws opsworks-cm create-backup ... --tags Key=Stage,Value=Production Key=Department,Value=Marketing
  ```

### Add Tags to Existing Servers or Backups \(CLI\)<a name="opscm-tags-howto-addtags"></a>

You can run the `tag-resource` command to add tags to existing AWS OpsWorks for Chef Automate servers or backups \(whether the backups were created automatically or manually\)\. Specify the Amazon Resource Number \(ARN\) of a target resource to add tags to it\.

1. To get the ARN of the resource to which you want to apply tags:
   + For a server, run `describe-servers --server-name server_name`\. The results of the command show the server ARN\.
   + For a backup, run `describe-backups --backup-id backup_ID`\. The results of the command show the backup ARN\. You can also run `describe-backups --server-name server_name` to show information about all backups for a specific AWS OpsWorks for Chef Automate server\.

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

1. To verify that tags were added successfully, go on to the next procedure, [List Resource Tags](#opscm-tags-howto-listtags)\.

### List Resource Tags<a name="opscm-tags-howto-listtags"></a>

You can run the `list-tags-for-resource` command to show the tags that are attached to AWS OpsWorks for Chef Automate servers or backups\. Specify the ARN of a target resource to view its tags\.

1. To get the ARN of the resource for which you want to list tags:
   + For a server, run `describe-servers --server-name server_name`\. The results of the command show the server ARN\.
   + For a backup, run `describe-backups --backup-id backup_ID`\. The results of the command show the backup ARN\. You can also run `describe-backups --server-name server_name` to show information about all backups for a specific AWS OpsWorks for Chef Automate server\.

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

### Delete Tags from a Resource<a name="opscm-tags-howto-untag"></a>

You can run the `untag-resource` command to delete tags from AWS OpsWorks for Chef Automate servers or backups\. If the resource is deleted, the tags on the resource are also deleted\. Specify the Amazon Resource Number \(ARN\) of a target resource to remove tags from it\.

1. To get the ARN of the resource from which you want to remove tags:
   + For a server, run `describe-servers --server-name server_name`\. The results of the command show the server ARN\.
   + For a backup, run `describe-backups --backup-id backup_ID`\. The results of the command show the backup ARN\. You can also run `describe-backups --server-name server_name` to show information about all backups for a specific AWS OpsWorks for Chef Automate server\.

1. Run the `untag-resource` command with the ARN that you returned in step 1\. Specify only the tags that you want to delete\.

   ```
   aws opsworks-cm untag-resource --resource-arn "server_or_backup_ARN" --tags Key=Key1,Value=Value1 Key=Key2,Value=Value2
   ```

   In this example, the `untag-resource` command removes only the tag with a key of `Stage` and a value of `Production`\.

   ```
   aws opsworks-cm untag-resource --resource-arn "arn:aws:opsworks-cm:us-west-2:123456789012:server/opsworks-cm-test/EXAMPLEd-66b0-4196-8274-d1a2bEXAMPLE" --tags Key=Stage,Value=Production
   ```

1. To verify that tags were deleted successfully, follow steps in [List Resource Tags](#opscm-tags-howto-listtags) in this topic\.

## See Also<a name="opscm-tags-seealso"></a>
+ [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli)
+ [Back Up an AWS OpsWorks for Chef Automate Server](opscm-chef-backup.md)
+ [AWS Tagging Strategies](http://aws.amazon.com/answers/account-management/aws-tagging-strategies/)
+ [Controlling Access to AWS Resources Using Resource Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *AWS Identity and Access Management User Guide*
+ [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*
+ [CreateBackup](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateBackup.html) in the *AWS OpsWorks CM API Reference*
+ [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) in the *AWS OpsWorks CM API Reference*
+ [TagResource](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_TagResource.html) in the *AWS OpsWorks CM API Reference*
+ [ListTagsForResource](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_ListTagsForResource.html) in the *AWS OpsWorks CM API Reference*
+ [UntagResource](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_UntagResource.html) in the *AWS OpsWorks CM API Reference*