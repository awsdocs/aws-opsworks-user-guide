# Step 3: Scale Out IISExample<a name="gettingstarted-windows-scale"></a>

If your incoming user requests start to approach the limit of what you can handle with a single t2\.micro instance, you will need to increase your server capacity\. You can move to a larger instance, but that has limits\. A more flexible approach is to add instances to your stack, and put them behind a load balancer\. The basic architecture looks something like the following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch_4_windows.png)

Among other advantages, this approach is much more robust than a single large instance\.
+ If one of your instances fails, the load balancer will distribute incoming requests to the remaining instances, and your application will continue to function\.
+ If you put instances in different Availability Zones \(the recommended practice\), your application will continue to function even if an Availability Zone encounters problems\.

AWS OpsWorks Stacks makes it easy to scale out stacks\. This section describes the basics of how to scale out a stack by adding a second 24/7 PHP App Server instance to IISExample and putting both instances behind an Elastic Load Balancing load balancer\. You can easily extend the procedure to add an arbitrary number of 24/7 instances, or you can use time\-based instances to have AWS OpsWorks Stacks scale your stack automatically\. For more information, see [Managing Load with Time\-based and Load\-based Instances](workinginstances-autoscaling.md)\. 