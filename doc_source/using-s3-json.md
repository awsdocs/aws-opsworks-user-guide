# Step 5: Add Access Information to the Stack Configuration and Deployment Attributes<a name="using-s3-json"></a>

The `appsetup.rb` recipe depends on data from the AWS OpsWorks Stacks [stack configuration and deployment attributes](workingcookbook-json.md), which are installed on each instance and contain detailed information about the stack and any deployed apps\. The object's `deploy` attributes have the following structure, which is displayed for convenience as JSON:

```
{
   ...
  "deploy": {
    "app1": {
      "application" : "short_name",
      ...
    }
    "app2": {
      ...
    }
    ...
  }
}
```

The deploy node contains an attribute for each deployed app that is named with the app's short name\. Each app attribute contains a set of attributes that define the app's configuration, such as the document root and app type\. For a list of the `deploy` attributes, see [deploy Attributes](attributes-json-deploy.md)\. You can represent stack configuration and deployment attribute values in your recipes by using Chef attribute syntax\. For example,`[:deploy][:app1][:application]` represents the app1 app's short name\. 

The custom recipes depend on several stack configuration and deployment attributes that represent database and Amazon S3 access information:
+ The database connection attributes, such as `[:deploy][:database][:host]`, are defined by AWS OpsWorks Stacks when it creates the MySQL layer\.
+ The table name attribute, `[:photoapp][:dbtable]`, is defined in the custom cookbook's attributes file, and is set to `foto`\.
+ You must define the bucket name attribute, `[:photobucket]`, by using custom JSON to add the attribute to the stack configuration and deployment attributes\.

**To define the Amazon S3 bucket name attribute**

1. On the AWS OpsWorks Stacks **Stack** page, choose** Stack Settings** and then **Edit**\.

1. In the **Configuration Management** section, add access information to the **Custom Chef JSON** box\. It should look something like the following:

   ```
   {
     "photobucket" : "yourbucketname"
   }
   ```

   Replace *yourbucketname* with the bucket name that you recorded in [Step 1: Create an Amazon S3 Bucket](using-s3-bucket.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/photoapp_walkthrough_json.png)

AWS OpsWorks Stacks merges the custom JSON into the stack configuration and deployment attributes before it installs them on the stack's instances; `appsetup.rb` can then obtain the bucket name from the `[:photobucket]` attribute\. If you want to change the bucket, you don't need to touch the recipe; you can just [override the attribute](workingcookbook-attributes.md) to provide a new bucket name\.