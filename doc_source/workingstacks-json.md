# Using Custom JSON<a name="workingstacks-json"></a>

Several AWS OpsWorks Stacks actions allow you to specify custom JSON, which AWS OpsWorks Stacks installs on instances and can be used by recipes\.

You can specify custom JSON in the following situations:
+ When you create, update, or clone a stack\.

  AWS OpsWorks Stacks installs the custom JSON on all instances for all subsequent [lifecycle events](workingcookbook-events.md)\.
+ When you run a deployment or stack command\.

  AWS OpsWorks Stacks passes the custom JSON to instances only for that event\.

Custom JSON must be represented by, and formatted as, a valid JSON object\. For example:

```
{
  "att1": "value1",
  "att2": "value2"
  ...
}
```

AWS OpsWorks Stacks stores custom JSON in the following locations:

On Linux instances:
+ `/var/chef/runs/run-ID/attribs.json`
+ `/var/chef/runs/run-ID/nodes/hostname.json`

On Windows instances:
+ `drive:\chef\runs\run-ID\attribs.json`
+ `drive:\chef\runs\run-ID\nodes\hostname.json`

**Note**  
In Chef 11\.10 and earlier versions for Linux, the custom JSON is located in the following path on Linux instances, Windows instances are not available, and there is no `attribs.json` file\. The logs are stored in the same folder or directory as the JSON\. For more information about custom JSON in Chef 11\.10 and earlier versions for Linux, see [Overriding Attributes with Custom JSON](http://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-json-override.html) and [Chef Logs](https://docs.aws.amazon.com/opsworks/latest/userguide/troubleshoot-debug-log.html#troubleshoot-debug-log-instance)\.  
`/var/lib/aws/opsworks/chef/hostname.json`

In the preceding paths, *run\-ID* is a unique ID that AWS OpsWorks Stacks assigns to each Chef run on an instance, and *hostname* is the instance's hostname\.

To access custom JSON from Chef recipes, use standard Chef `node` syntax\.

For example, suppose that you want to define simple settings for an app that you want to deploy, such as whether the app is initially visible and the app's initial foreground and background colors\. Suppose you define these app settings with a JSON object as follows: 

```
{
  "state": "visible",
  "colors": {
    "foreground": "light-blue",
    "background": "dark-gray"
  }
}
```

To declare the custom JSON for a stack:

1. On the stack page, choose **Stack Settings**, and then choose **Edit**\.

1. For **Custom Chef JSON**, type the JSON object, and then choose **Save**\.

**Note**  
You can declare custom JSON at the deployment, layer, and stack levels\. You may want to do this if you want some custom JSON to be visible only to an individual deployment or layer\. Or, for example, you may want to temporarily override custom JSON declared at the stack level with custom JSON declared at the layer level\. If you declare custom JSON at multiple levels, custom JSON declared at the deployment level overrides any custom JSON declared at both the layer and stack levels\. Custom JSON declared at the layer level overrides any custom JSON declared only at the stack level\.  
To use the AWS OpsWorks Stacks console to specify custom JSON for a deployment, on the **Deploy App** page, choose **Advanced**\. Type the custom JSON in the **Custom Chef JSON** box, and then choose **Save**\.  
To use the AWS OpsWorks Stacks console to specify custom JSON for a layer, on the **Layers** page, choose **Settings** for the desired layer\. Type the custom JSON in the **Custom JSON** box, and then choose **Save**\.  
For more information, see [Editing an OpsWorks Layer's Configuration](workinglayers-basics-edit.md) and [Deploying Apps](workingapps-deploying.md)\.

When you run a deployment or stack command, recipes can retrieve these custom values by using standard Chef `node` syntax, which maps directly to the hierarchy in the custom JSON object\. For example, the following recipe code writes messages to the Chef log about the preceding custom JSON values:

```
Chef::Log.info("********** The app's initial state is '#{node['state']}' **********")
Chef::Log.info("********** The app's initial foreground color is '#{node['colors']['foreground']}' **********")
Chef::Log.info("********** The app's initial background color is '#{node['colors']['background']}' **********")
```

This approach can be useful for passing data to recipes\. AWS OpsWorks Stacks adds that data to the instance, and recipes can retrieve the data by using standard Chef `node` syntax\. 

**Note**  
Custom JSON is limited to 120 KB\. If you need more capacity, we recommend storing some of the data on Amazon Simple Storage Service \(Amazon S3\)\. Your custom recipes can then use the [AWS CLI](http://aws.amazon.com/documentation/cli/) or the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/) to download the data from the Amazon S3 bucket to your instance\.