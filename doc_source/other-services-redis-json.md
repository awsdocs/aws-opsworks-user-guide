# Step 5: Add Access Information to the Stack Configuration JSON<a name="other-services-redis-json"></a>

The `generate.rb` recipe depends on a pair of stack configuration and deployment JSON attributes that represent the Redis server's host name and port\. Although these attributes are part of the standard `[:deploy]` namespace, they are not automatically defined by AWS OpsWorks Stacks\. Instead, you define the attributes and their values by adding a custom JSON object to the stack\. The following example shows the custom JSON for this example\.

**To add access information to the stack configuration and deployment JSON**

1. On the AWS OpsWorks Stacks **Stack** page, click** Stack Settings** and then **Edit**\.

1. In the **Configuration Management** section, add access information to the **Custom Chef JSON** box\. It should look something like the following example, with these modifications:
   + Replace `elasticache_redis_example` with your app's short name\. 
   + Replace the `host` and `port` values with the values for the ElastiCache Redis server instance that you created in [Step 1: Create an ElastiCache Redis Cluster](other-services-redis-cluster.md)\.

   ```
   {
     "deploy": {
        "elasticache_redis_example": {
          "redis": {
            "host": "mycluster.XXXXXXXXX.amazonaws.com",
            "port": "6379"
          }
        }
     }
   }
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/redis_walkthrough_json.png)

The advantage of this approach is that you can change the port or host value at any time without touching your custom cookbook\. AWS OpsWorks Stacks merges custom JSON into the built\-in JSON and installs it on the stack's instances for all subsequent lifecycle events\. Apps can then access the attribute values by using Chef node syntax, as described in [Step 3: Create and Deploy a Custom Cookbook](other-services-redis-cookbook.md)\. The next time you deploy an app, AWS OpsWorks Stacks will install a stack configuration and deployment JSON that contains the new definitions, and `generate.rb` will create a configuration file with the updated host and port values\.

**Note**  
`[:deploy]` automatically includes an attribute for every deployed app, so `[:deploy][elasticache_redis_example]` is already in the stack and configuration JSON\. However, `[:deploy][elasticache_redis_example]` does not include a `[:redis]` attribute, defining them with custom JSON directs AWS OpsWorks Stacks to add those attributes to `[:deploy][elasticache_redis_example]`\. You can also use custom JSON to override existing attributes\. For more information, see [Overriding Attributes](workingcookbook-attributes.md)\. 