# Monitoring Stacks using Amazon CloudWatch Events<a name="monitoring-cloudwatch-events"></a>

You can configure rules in Amazon CloudWatch Events to alert you to changes in AWS OpsWorks Stacks resources, and direct CloudWatch Events to take actions based on event contents\. For more information about how to get started with CloudWatch Events and set up rules, see [Getting Started with CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_GettingStarted.html) in the *CloudWatch Events User Guide*\.

The following AWS OpsWorks Stacks event types are supported in CloudWatch Events\.

**Instance state change**  
Indicates a change in the state of an AWS OpsWorks Stacks instance\.

**Command state change**  
Indicates a change occurred in the state of an AWS OpsWorks Stacks command\.

**Deployment state change**  
Indicates a change occurred in the state of an AWS OpsWorks Stacks deployment\.

**Alerts**  
Indicates an AWS OpsWorks Stacks service error was raised\.

For more information about the AWS OpsWorks Stacks event types that are supported by CloudWatch Events, see [AWS OpsWorks Stacks Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#opsworks_event_types) in the *CloudWatch Events User Guide*\.