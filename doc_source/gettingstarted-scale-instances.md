# Step 4\.2: Add PHP App Server Instances<a name="gettingstarted-scale-instances"></a>

Now the load balancer is in place, you can scale out the stack by adding more instances to the PHP App Server layer\. From your perspective, the operation is seamless\. Each time a new PHP App Server instance comes online, AWS OpsWorks Stacks automatically registers it with the load balancer and deploys SimplePHPApp, so the server can immediately start handling incoming traffic\. For brevity, this topic shows how to add one additional PHP App Server instance, but you can use the same approach to add as many as you need\.

**To add another instance to the PHP App Server layer**

1. On the Instances page, click **\+ Instance** under **PHP App Server**\.

1. Accept the default settings and click **Add Instance**\.

1. Click **start** to start the instance\.