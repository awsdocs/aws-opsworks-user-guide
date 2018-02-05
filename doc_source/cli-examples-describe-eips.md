# List a Stack's Elastic IP Addresses \(describe\-elastic\-ips\)<a name="cli-examples-describe-eips"></a>

Use the [describe\-elastic\-ips](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-elastic-ips.html) command to list the Elastic IP addresses that have been registered with a stack or get details about specified Elastic IP addresses\.

```
aws opsworks --region us-west-2 describe-elastic-ips --instance-id b62f3e04-e9eb-436c-a91f-d9e9a396b7b0
```

The preceding command returns a JSON object that contains details about each Elastic IP address \(one in this example\) for a specified instance\. For a description of each parameter, see [describe\-elastic\-ips](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-elastic-ips.html)\.

```
{
  "ElasticIps": [
      {
          "Ip": "192.0.2.0",
          "Domain": "standard",
          "Region": "us-west-2"
      }
  ]
}
```