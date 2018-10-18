# Execute a Recipe \(create\-deployment\)<a name="cli-examples-execute-recipe"></a>

Use the [create\-deployment](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-deployment.html) command to execute [stack commands](workingstacks-commands.md) and [deployment commands](workingapps-deploying.md)\. The following example executes a stack command to run a custom recipe on a specified stack\.

```
aws opsworks --region us-west-1 create-deployment --stack-id 935450cc-61e0-4b03-a3e0-160ac817d2bb
    --command "{\"Name\":\"execute_recipes\", \"Args\":{\"recipes\":[\"phpapp::appsetup\"]}}"
```

The `command` argument takes a JSON object that is formatted as follows: 
+ `Name` \- Specifies the command name\. The `execute_recipes` command used for this example executes a specified recipe on the stack's instances\.
+ `Args` \- Specifies a list of arguments and their values\. This example has one argument, `recipes`, which is set to the recipe to be executed, `phpapp::appsetup`\.

Notice that the `"` characters in the JSON object are all escaped\. Otherwise, the command might return an error that the JSON is invalid\.

The command returns a deployment ID, which you can use to identify the command for other CLI commands such as `describe-commands`\.

```
{
    "DeploymentId": "5cbaa7b9-4e09-4e53-aa1b-314fbd106038"
}
```