# Tags<a name="tagging"></a>

Tags can help you group resources in Chef 11\.10, Chef 12, and Chef 12\.2 stacks, and track the costs of using those resources in [AWS Billing and Cost Management](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html)\.

You can apply tags at the stack and layer level\. When you create a tag, you are applying the tag to every resource within the tagged structure\. For example, if you apply a tag to a layer, you are applying the tag to every instance, Amazon EBS volume \(except the root\), or Elastic Load Balancing load balancer in the layer\. Tags cannot currently be applied to the root, or default, EBS volume of an instance\.

Tags are key\-value pairs that you assign to stacks or layers in AWS OpsWorks Stacks\. After you create tags, open the Billing and Cost Management console to activate user\-defined tags\. For more information about how to activate your tags and use them to track and manage the costs of your AWS OpsWorks Stacks resources, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) and [Activating User\-Defined Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html) in the *Billing and Cost Management User Guide*\.

Tags work in a way that's similar to custom attributes in AWS OpsWorks Stacks\. Tags that you apply to a stack are inherited by each layer in the stack\. At the layer level, you can override the values \(but not the key names\) of inherited tags, and add new layer\-specific tags\. AWS OpsWorks applies the resulting tag set to all resources in the layer\. As you create new or assign existing resources to a layer, the new resources in the layer are tagged with the same set of tags\.

**Topics**
+ [Setting Tags at the Stack Level](#w4ab1c11c55c13)
+ [Setting Tags at the Layer Level](#w4ab1c11c55c15)
+ [Managing Tags with the AWS CLI](#w4ab1c11c55c17)
+ [Tag Limitations](#w4ab1c11c55c19)

## Setting Tags at the Stack Level<a name="w4ab1c11c55c13"></a>

At the stack level, you can add and manage tags by choosing **Tags** on the stack's home page\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/stack_tags.png)

On the **Tags** page, add tags as key\-value pairs\. The following screenshot shows some example tags\. You can delete tags by choosing the red **X** to the right of a key\-value pair\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/stack_tags_add.png)

## Setting Tags at the Layer Level<a name="w4ab1c11c55c15"></a>

At the layer level, set tags by choosing the **Tags** tab\. You can find this tab on the **Layers** home page, and the home page for each individual layer\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/layers_tags.png)

When you change or add tags at the layer level, be aware that tags that have been added at the parent stack level are inherited by the layer and its resources\. While you can change the values of inherited tags, you cannot change the key names, or delete inherited tags\. Change the key names or delete tags inherited from the parent stack in stack settings\. The following screenshot shows examples of tags inherited from the stack level\. Inherited tags are grayed out\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/layer_inherited_tags.png)

For more information about adding tags to stacks, see [Create a New Stack](workingstacks-creating.md)\. For more information about adding tags to layers, see [Editing an OpsWorks Layer's Configuration](workinglayers-basics-edit.md)\.

## Managing Tags with the AWS CLI<a name="w4ab1c11c55c17"></a>

You can also use AWS CLI commands to add and remove tags at the stack and layer level\. For more information about downloading and installing the AWS CLI, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\. Remember to add the `--region` parameter to your command if the stack that you want to tag is not in your default region\. Layer ARNs do not currently appear in the AWS Management Console\. To get the ARN of a layer, run the [describe\-layers](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-layers.html) command\.

**To add tags by using the AWS CLI**
+ At the AWS CLI command prompt, type the following command, replacing *stack\_or\_layer\_ARN* and specifying your key\-value pair tags, and then press **Enter**\. Double quotation marks are escaped with backslashes\. 

  ```
  aws opsworks tag-resource --resource-arn stack_or_layer_ARN --tags "{\"key\":\"value\",\"key\":\"value\"}"
  ```

  The following is an example\.

  ```
  aws opsworks tag-resource --resource-arn arn:aws:opsworks:us-east-2:800000000003:stack/500b99c0-ec00-4cgg-8a0d-1000000jjd1b --tags "{\"Stage\":\"Production\",\"Organization\":\"Mobile\"}"
  ```

**To remove tags by using the AWS CLI**
+ At the AWS CLI command prompt, type the following, and then press **Enter**\.

  ```
  aws opsworks untag-resource --resource-arn stack_or_layer_ARN --tag-keys "[\"key\",\"key\"]"
  ```

  To remove tags, you only specify the key of the tag that you want to remove\. The following is an example\.

  ```
  aws opsworks untag-resource --resource-arn arn:aws:opsworks:us-east-2:800000000003:stack/500b99c0-ec00-4cgg-8a0d-1000000jjd1b --tag-keys "[\"Stage\",\"Organization\"]"
  ```
**Note**  
You cannot remove inherited tags \(tags that were added at the parent stack level\) from a layer\. Remove inherited tags from the stack instead\.

## Tag Limitations<a name="w4ab1c11c55c19"></a>

Keep the following limitations in mind when you create tags\.
+ AWS OpsWorks Stacks limits the number of user\-defined tags at the stack and layer level to 40, including user\-defined tags inherited from a parent level\. This leaves 10 available slots for default tags that are prepended with `opsworks:`, and tags that are set by other AWS processes\. A maximum of 50 tags is allowed on a resource, including both user\-defined and default tags that are created by AWS\.
+ Tag keys cannot start with **aws:**, **opsworks:** or **rds:**\. Do not use **name** or **Name** as a tag key, because **Name** is reserved by AWS OpsWorks Stacks\.
+ A key can be a maximum of 127 characters, and can contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / `\.
+ A value can be a maximum 255 characters, and contain only Unicode letters, numbers, or separators, or the following special characters: `+ - = . _ : / `\.