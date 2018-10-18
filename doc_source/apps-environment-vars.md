# Using Environment Variables<a name="apps-environment-vars"></a>

**Note**  
The recommendations in this topic apply to Chef 11\.10 and earlier versions of Chef\. To get environment variables in Chef 12 and newer releases, you must use the App Data Bag\. For more information, see [AWS OpsWorks Data Bag Reference](http://docs.aws.amazon.com/opsworks/latest/userguide/data-bags.html) and [App Data Bag \(aws\_opsworks\_app\)](http://docs.aws.amazon.com/opsworks/latest/userguide/data-bag-json-app.html)\.

When you [specify environment variables for an app](workingapps-creating.md#workingapps-creating-environment), AWS OpsWorks Stacks adds the variable definitions to the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy)\.

Custom layers can use a recipe to retrieve a variable's value by using standard node syntax, and store it in a form that is accessible to the layer's apps\.

You must implement a custom recipe that obtains the environment variable values from the instance's `deploy` attributes\. The recipe can then store the data on the instance in a form that can be accessed by the application, such as a YAML file\. An app's environment variable definitions are stored in the `deploy` attributes, in the app's `environment_variables`\. The following example shows the location of these attributes for an app named `simplephpapp`, using JSON to represent the attribute structure\.

```
{
  ...
  "ssh_users": {
  },
  "deploy": {
    "simplephpapp": {
      "application": "simplephpapp",
      "application_type": "php",
      "environment_variables": {
        "USER_ID": "168424",
        "USER_KEY": "somepassword"
      },
    ...
  }
}
```

A recipe can obtain variable values by using standard node syntax\. The following example shows how to obtain the `USER_ID` value from the preceding JSON and place it in the Chef log\.

```
Chef::Log.info("USER_ID: #{node[:deploy]['simplephpapp'][:environment_variables][:USER_ID]}")
```

For a more detailed description of how to retrieve information from the stack configuration and deployment JSON and store it on the instance, see [Passing Data to Applications](apps-data.md)\.