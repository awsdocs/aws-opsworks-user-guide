# Deploy an App \(create\-deployment\)<a name="cli-examples-create-deployment"></a>

Use the [create\-deployment](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-deployment.html) command to deploy an app to a specified stack\.

**Topics**
+ [Deploy an App](#cli-examples-create-deployment-deploy)

## Deploy an App<a name="cli-examples-create-deployment-deploy"></a>

```
aws opsworks --region us-west-1 create-deployment --stack-id cfb7e082-ad1d-4599-8e81-de1c39ab45bf
   --app-id 307be5c8-d55d-47b5-bd6e-7bd417c6c7eb --command "{\"Name\":\"deploy\"}"
```

The arguments are as follows:
+ `stack-id` – You can get the stack ID from the stack's settings page on the console \(look for **OpsWorks ID**\) or by calling `describe-stacks`\.
+ `app-id` – You can get app ID from the app's details page \(look for **OpsWorks ID**\) or by calling [describe\-apps](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-apps.html)\.
+ `command` – The argument takes a JSON object that sets the command name to `deploy`, which deploys the specified app to the stack\. 

  Notice that the `"` characters in the JSON object are all escaped\. Otherwise, the command might return an error that the JSON is invalid\.

The command returns a JSON object that contains the deployment ID, as follows:

```
{
    "DeploymentId": "5746c781-df7f-4c87-84a7-65a119880560"
}
```

**Note**  
The preceding example deploys to every instance in the stack\. To deploy to a specified subset of instances, add an `instance-ids` argument and list the instance IDs\.