# Create an Instance \(create\-instance\)<a name="cli-examples-create-instance"></a>

Use the [create\-instance](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-instance.html) command to create an instance on a specified stack\.

**Topics**
+ [Create an Instance with a Default Host Name](#cli-examples-create-instance-default)
+ [Create an Instance with a Themed Host Name](#cli-examples-create-instance-themed)
+ [Create an Instance with a Custom AMI](#cli-examples-create-instance-custom-ami)

## Create an Instance with a Default Host Name<a name="cli-examples-create-instance-default"></a>

```
C:\>aws opsworks --region us-west-1 create-instance --stack-id 935450cc-61e0-4b03-a3e0-160ac817d2bb
    --layer-ids 5c8c272a-f2d5-42e3-8245-5bf3927cb65b --instance-type m1.large --os "Amazon Linux"
```

The arguments are as follows:
+ `stack-id` – You can get the stack ID from the stack's settings page on the console \(look for **OpsWorks ID**\) or by calling [describe\-stacks](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-stacks.html)\.
+ `layer-ids` – You can get layer IDs from the layer's details page on the console \(look for **OpsWorks ID**\) or by calling [describe\-layers](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-layers.html)\. In this example, the instance belongs to only one layer\.
+ `instance-type` – The specification that defines the memory, CPU, storage capacity, and hourly cost for the instance; `m1.large` for this example\.
+ `os` – The instance's operating system; Amazon Linux for this example\.

The command returns a JSON object that contains the instance ID, as follows:

```
{
    "InstanceId": "5f9adeaa-c94c-42c6-aeef-28a5376002cd"
}
```

This example creates an instance with a default host name, which is simply an integer\. The following section describes how to create an instance with a host name generated from a theme\. 

## Create an Instance with a Themed Host Name<a name="cli-examples-create-instance-themed"></a>

You can also create an instance with a themed host name\. You specify the theme when you create the stack\. For more information, see [Create a New Stack](workingstacks-creating.md)\.To create the instance, first call [get\-hostname\-suggestion](http://docs.aws.amazon.com/cli/latest/reference/opsworks/get-hostname-suggestion.html) to generate a name\. For example:

```
C:\>aws opsworks get-hostname-suggestion --region us-west-1 --layer-id 5c8c272a-f2d5-42e3-8245-5bf3927cb65b
```

If you specify the default `Layer Dependent` theme, `get-hostname-suggestion` simply appends a digit to the layer's short name\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

The command returns the generated host name\.

```
{
    "Hostname": "php-app2",
    "LayerId": "5c8c272a-f2d5-42e3-8245-5bf3927cb65b"
}
```

You can then use the `hostname` argument to pass the generated name to `create-instance`, as follows:

```
c:\>aws --region us-west-1 opsworks create-instance --stack-id 935450cc-61e0-4b03-a3e0-160ac817d2bb
   --layer-ids 5c8c272a-f2d5-42e3-8245-5bf3927cb65b --instance-type m1.large --os "Amazon Linux" --hostname "php-app2"
```

## Create an Instance with a Custom AMI<a name="cli-examples-create-instance-custom-ami"></a>

The following [create\-instance](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-instance.html) command creates an instance with a custom AMI, which must be from the stack's region\. For more information about how to create a custom AMI for AWS OpsWorks Stacks, see [Using Custom AMIs](workinginstances-custom-ami.md)\.

```
C:\>aws opsworks create-instance --region us-west-1 --stack-id c5ef46ce-3ccd-472c-a3de-9bec94c6028e
   --layer-ids 6ff8a2ac-c9cc-49cf-9c67-fc852539ade4 --instance-type c3.large --os Custom
   --ami-id ami-6c61f104
```

The arguments are as follows:
+ `stack-id` – You can get the stack ID from the stack's settings page on the console \(look for **OpsWorks ID**\) or by calling [describe\-stacks](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-stacks.html)\.
+ `layer-ids` – You can get layer IDs from the layer's details page on the console \(look for **OpsWorks ID**\) or by calling [describe\-layers](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-layers.html)\. In this example, the instance belongs to only one layer\.\.
+ `instance-type` – The value defines the instance's memory, CPU, storage capacity, and hourly cost, and must be compatible with the AMI \(`c3.large` for this example\)\.
+ `os` – The instance's operating system, which must be set to `Custom` for a custom AMI\.
+ `ami-id` – The AMI ID, which should look something like `ami-6c61f104`

**Note**  
When you use a custom AMI, block device mappings are not supported, and values that you specify for the `--block-device-mappings` option are ignored\.

The command returns a JSON object that contains the instance ID, as follows:

```
{
    "InstanceId": "5f9adeaa-c94c-42c6-aeef-28a5376002cd"
}
```