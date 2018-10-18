# Overriding Attributes With Custom JSON<a name="workingcookbook-json-override"></a>

**Note**  
Because AWS OpsWorks Stacks handles Chef runs differently for Windows stacks than for Linux stacks, you cannot use the techniques discussed in this section for Windows stacks\.

The simplest way to override an AWS OpsWorks Stacks attribute is to define it in custom JSON, which takes precedence over stack configuration and deployment attributes as well as built\-in and custom cookbook `default` attributes\. For more information, see [Attribute Precedence](workingcookbook-attributes-precedence.md)\.

**Important**  
You should override stack configuration and deployment attributes with care\. For example overriding attributes in the `opsworks` namespace can interfere with the built\-in recipes\. For more information, see [Stack Configuration and Deployment Attributes](workingcookbook-json.md)\.

You can also use custom JSON to define unique attributes, typically to pass data to your custom recipes\. The attributes are simply incorporated into the node object, and recipes can reference them by using the standard Chef node syntax\.

## How to Specify Custom JSON<a name="workingcookbook-json-override-specify"></a>

To use custom JSON to override an attribute value, you must first determine the attribute's fully qualified attribute name\. You then create a JSON object that contains the attributes you want to override, set to your preferred values\. For convenience, [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md) and [Built\-in Cookbook Attributes](attributes-recipes.md) documents commonly used stack configuration, deployment, and built\-in cookbook attributes, including their fully qualified names\.

The object's parent\-child relationships must correspond to the appropriate fully qualified Chef nodes\. For example, suppose you want to change the following Apache attributes: 
+ The [`keepalivetimeout`](attributes-recipes-apache.md#attributes-recipes-apache-keep-timeout) attribute, whose node is `node[:apache][:keepalivetimeout]` and has a default value of `3`\.
+ The `logrotate` [`schedule`](attributes-recipes-apache.md#attributes-recipes-apache-log-schedule) attribute, whose node is `node[:apache][:logrotate][:schedule]`, and has a default value of `"daily"`\.

To override the attributes and set the values to `5` and `"weekly"`, respectively, you would use the following custom JSON:

```
{
  "apache" : {
    "keepalivetimeout" : 5,
    "logrotate" : {
       "schedule" : "weekly"
    }
  }
}
```

## When to Specify Custom JSON<a name="workingcookbook-json-override-when"></a>

You can specify a custom JSON structure for the following tasks:
+ [Create a new stack](workingstacks-creating.md)
+ [Update a stack](workingstacks-edit.md)
+ [Run a stack command](workingstacks-edit.md)
+ [Clone a stack](workingstacks-cloning.md)
+ [Deploy an app](workingapps-deploying.md)

For each task, AWS OpsWorks Stacks merges the custom JSON attributes with the stack configuration and deployment attributes and sends it to the instances, to be merged into the node object\. However, note the following:
+ If you specify custom JSON when you create, clone, or update a stack, the attributes are merged into the stack configuration and deployment attributes for all subsequent lifecycle events and stack commands\.
+ If you specify custom JSON for a deployment, the attributes are merged into the stack configuration and deployment attributes only for the corresponding event\.

  If you want to use those custom attributes for subsequent deployments, you must explicitly specify the custom JSON again\.

It is important to remember that attributes only affect the instance when they are used by recipes\. If you override an attribute value but no subsequent recipes reference the attribute, the change has no effect\. You must either ensure that the custom JSON is sent before the associated recipes run, or ensure that the appropriate recipes are re\-run\. 

## Custom JSON Best Practices<a name="workingcookbook-json-override-best"></a>

You can use custom JSON to override any AWS OpsWorks Stacks attribute, but manually entering the information is somewhat cumbersome, and it is not under any sort of source control\. Custom JSON is best used for the following purposes:
+ When you want to override only a small number of attributes, and you do not otherwise need to use custom cookbooks\.

  With custom JSON, you can avoid the overhead of setting up and maintaining a cookbook repository just to override a couple of attributes\.
+ Sensitive values, such as passwords or authentication keys\.

  Cookbook attributes are stored in a repository, so any sensitive information is at some risk of being compromised\. Instead, define attributes with dummy values and use custom JSON to set the real values\.
+ Values that are expected to vary\.

  For example, a recommended practice is to have your production stack supported by separate development and staging stacks\. Suppose that these stacks support an application that accepts payments\. If you use custom JSON to specify the payment endpoint, you can specify a test URL for your staging stack\. When you are ready to migrate an updated stack to your production stack, you can use the same cookbooks and use custom JSON to set the payment endpoint to the production URL\.
+ Values that are specific to a particular stack or deployment command\.