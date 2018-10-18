# Update the Stack Configuration \(update\-stack\)<a name="cli-examples-update-stack"></a>

Use the [update\-stack](http://docs.aws.amazon.com/cli/latest/reference/opsworks/update-stack.html) command to update the configuration of a specified stack\. The following example updates a stack to add custom JSON to the [stack configuration attributes](workingstacks-json.md)\.

```
aws opsworks --region us-west-1 update-stack --stack-id 935450cc-61e0-4b03-a3e0-160ac817d2bb
   --custom-json "{\"somekey\":\"somevalue\"}" --service-role-arn arn:aws:iam::444455556666:role/aws-opsworks-service-role
```

Notice that the `"` characters in the JSON object are all escaped\. Otherwise, the command might return an error that the JSON is invalid\.

**Note**  
The example also specifies a service role for the stack\. You must set `service-role-arn` to a valid service role ARN or the action will fail; there is no default value\. You can specify the stack's current service role ARN, if you prefer, but you must do so explicitly\.

The `update-stack` command does not return a value\.