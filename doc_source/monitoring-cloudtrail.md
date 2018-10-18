# Logging AWS OpsWorks Stacks API Calls with AWS CloudTrail<a name="monitoring-cloudtrail"></a>

AWS OpsWorks Stacks is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in AWS OpsWorks Stacks\. CloudTrail captures all API calls for AWS OpsWorks Stacks as events, including calls from the AWS OpsWorks Stacks console and from code calls to the AWS OpsWorks Stacks APIs\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for AWS OpsWorks Stacks\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to AWS OpsWorks Stacks, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## AWS OpsWorks Stacks Information in CloudTrail<a name="opsworks-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in AWS OpsWorks Stacks, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for AWS OpsWorks Stacks, create a trail\. A trail enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all regions\. The trail logs events from all regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

All AWS OpsWorks Stacks actions are logged by CloudTrail and are documented in the [AWS OpsWorks Stacks API Reference\.](https://docs.aws.amazon.com/opsworks/latest/APIReference/Welcome.html) For example, calls to the `[CreateLayer](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateLayer.html)`, `[DescribeInstances](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeInstances.html)`, and `[StartInstance](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_StartInstance.html)` actions generate entries in the CloudTrail log files\.

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Understanding AWS OpsWorks Stacks Log File Entries<a name="understanding-opsworks-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

The following example shows a CloudTrail log entry that demonstrates the `CreateLayer` action\.

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