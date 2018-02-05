# Logging AWS OpsWorks Stacks API Calls By Using AWS CloudTrail<a name="monitoring-cloudtrail"></a>

AWS OpsWorks Stacks is integrated with CloudTrail, a service that captures API calls made by or on behalf of AWS OpsWorks Stacks in your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls from the AWS OpsWorks Stacks console or from the AWS OpsWorks Stacks API\. Using the information collected by CloudTrail, you can determine what request was made to AWS OpsWorks Stacks, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the [http://docs.aws.amazon.com/awscloudtrail/latest/userguide/](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## AWS OpsWorks Stacks Information in CloudTrail<a name="monitoring-cloudtrail-information"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to AWS OpsWorks Stacks actions are tracked in log files\. AWS OpsWorks Stacks records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

**Important**  
Although your stacks can be in any AWS region, all calls made by AWS OpsWorks Stacks on your behalf originate in the US East \(N\. Virginia\) Region\. To log API calls made on your behalf to other regions, you must enable CloudTrail in those regions\.

All of the AWS OpsWorks Stacks actions are logged and are documented in the [AWS OpsWorks Stacks API Reference](http://docs.aws.amazon.com/opsworks/latest/APIReference/Welcome.html)\. For example, each call to [CreateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateLayer.html), [DescribeInstances](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeInstances.html) or [StartInstance](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_StartInstance.html) generates a corresponding entry in the CloudTrail log files\. 

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the **userIdentity** field in the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted by using Amazon S3 server\-side encryption \(SSE\)\.

You can choose to have CloudTrail publish Amazon SNS notifications when new log files are delivered if you want to take quick action upon log file delivery\. For more information, see [Configuring Amazon SNS Notifications](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate AWS OpsWorks Stacks log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Aggregating CloudTrail Log Files to a Single Amazon S3 Bucket](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html)\.

## Understanding AWS OpsWorks Stacks Log File Entries<a name="monitoring-cloudtrail-log"></a>

CloudTrail log files can contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered stack trace of the public API calls\.

The following example shows a CloudTrail log entry that demonstrates the [CreateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateLayer.html) and [DescribeInstances](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeInstances.html) actions\.

**Note**  
Potentially sensitive information, including database passwords and custom JSON attributes, is redacted and does not appear in the CloudTrail logs\. The information is instead represented by `**redacted**`\.

```
      {
    "Records": [
        {
            "awsRegion": "us-west-2", 
            "eventID": "342cd1ec-8214-4a0f-a68f-8e6352feb5af", 
            "eventName": "CreateLayer", 
            "eventSource": "opsworks.amazonaws.com", 
            "eventTime": "2014-05-28T16:05:29Z", 
            "eventVersion": "1.01"ed, 
            "requestID": "e3952a2b-e681-11e3-aa71-81092480ee2e", 
            "requestParameters": {
                "attributes": {}, 
                "customRecipes": {}, 
                "name": "2014-05-28 16:05:29 +0000 a073", 
                "shortname": "customcf4571d5c0d6", 
                "stackId": "a263312e-f937-4949-a91f-f32b6b641b2c", 
                "type": "custom"
            }, 
            "responseElements": null, 
            "sourceIPAddress": "198.51.100.0", 
            "userAgent": "aws-sdk-ruby/2.0.0 ruby/2.1 x86_64-linux", 
            "userIdentity": {
                "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
                "accountId": "111122223333", 
                "arn": "arn:aws:iam::111122223333:user/A-User-Name", 
                "principalId": "AKIAI44QH8DHBEXAMPLE",
                "type": "IAMUser", 
                "userName": "A-User-Name"
            }
        }, 
        {
            "awsRegion": "us-west-2", 
            "eventID": "a860d8f8-c1eb-449b-8f55-eafc373b49a4", 
            "eventName": "DescribeInstances", 
            "eventSource": "opsworks.amazonaws.com", 
            "eventTime": "2014-05-28T16:05:31Z", 
            "eventVersion": "1.01", 
            "requestID": "e4691bfd-e681-11e3-aa71-81092480ee2e", 
            "requestParameters": {
                "instanceIds": [
                    "218289c4-0492-473d-a990-3fbe1efa25f6"
                ]
            }, 
            "responseElements": null, 
            "sourceIPAddress": "198.51.100.0", 
            "userAgent": "aws-sdk-ruby/2.0.0 ruby/2.1x86_64-linux", 
            "userIdentity": {
                "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
                "accountId": "111122223333", 
                "arn": "arn:aws:iam::111122223333:user/A-User-Name", 
                "principalId": "AKIAI44QH8DHBEXAMPLE", 
                "type": "IAMUser", 
                "userName": "A-User-Name"
            }
        } 
    ]
}
```