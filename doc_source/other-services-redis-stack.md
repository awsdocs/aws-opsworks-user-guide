# Step 2: Set up a Rails Stack<a name="other-services-redis-stack"></a>

In addition to creating a stack that supports a Rails App Server layer, you must also configure the layer's security groups so that the Rails server can communicate properly with Redis server\.

**To set up a stack**

1. Create a new stack—named **RedisStack** for this example—and add a Rails App Server layer\. You can use the default settings for both\. For more information, see [Create a New Stack](workingstacks-creating.md) and [Creating an OpsWorks Layer ](workinglayers-basics-create.md)\.

1. On the **Layers** page, for Rails App Server, click **Security** and then click **Edit**\.

1. Go to the **Security Groups** section and add the ElastiCache cluster's security group to **Additional groups**\. For this example, select the **default** security group, click **\+** to add it to the layer, and click **Save** to save the new configuration\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/redis-security.png)

1. Add an instance to the Rails App Server layer and start it\. For more information on how to add and start instances, see [Adding an Instance to a Layer](workinginstances-add.md)\.