# Step 1: Create an ElastiCache Redis Cluster<a name="other-services-redis-cluster"></a>

You must first create an Amazon ElastiCache Redis cluster by using the ElastiCache console, API, or CLI\. The following describes how to use the console to create a cluster\.

**To create an ElastiCache Redis cluster**

1. Go to the [ElastiCache console](https://console.aws.amazon.com/elasticache/) and click **Launch Cache Cluster** to start the Cache Cluster wizard\.

1. On the Cache Cluster Details page, do the following:
   + Set **Name** to your cache server name\.

     This example uses OpsWorks\-Redis\.
   + Set **Engine** to **redis**\.
   + Set **Topic for SNS Notification** to **Disable Notifications**\.
   + Accept the defaults for the other settings and click **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elasticache-wizard-1.png)

1. On the **Additional Configuration** page, accept the defaults and click **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elasticache-wizard-2.png)

1. Click **Launch Cache Cluster** to create the cluster\.
**Important**  
The default cache security group is sufficient for this example, but for production use you should create one that is appropriate for your environment\. For more information, see [Managing Cache Security Groups](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/ManagingSecurityGroups.html)\.

1. After the cluster has started, click the name to open the details page and click the **Nodes** tab\. Record the cluster's **Port** and **Endpoint** values for later use\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elasticache-wizard-3.png)