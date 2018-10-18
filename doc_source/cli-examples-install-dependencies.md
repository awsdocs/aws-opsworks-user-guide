# Install Dependencies \(create\-deployment\)<a name="cli-examples-install-dependencies"></a>

Use the [create\-deployment](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-deployment.html) command to execute [stack commands](workingstacks-commands.md) and [deployment commands](workingapps-deploying.md)\. The following example runs the `update_dependencies` stack command to update the dependencies on a stack's instances\.

```
aws opsworks --region us-west-1 create-deployment --stack-id 935450cc-61e0-4b03-a3e0-160ac817d2bb 
--command "{\"Name\":\"install_dependencies\"}"
```

The `command` argument takes a JSON object with a `Name` parameter whose value specifies the command name, `install_dependencies` for this example\. Notice that the `"` characters in the JSON object are all escaped\. Otherwise, the command might return an error that the JSON is invalid\.

The command returns a deployment ID, which you can use to identify the command for other CLI commands such as `describe-commands`\.

```
{
    "DeploymentId": "aef5b255-8604-4928-81b3-9b0187f962ff"
}
```