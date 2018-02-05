# Step 4: Scale Out MyStack<a name="gettingstarted-scale"></a>

MyStack currently has only one application server\. A production stack will probably need multiple application servers to handle the incoming traffic and a load balancer to distribute the incoming traffic evenly across the application servers\. The architecture will look something like the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch_4.png)

AWS OpsWorks Stacks makes it easy to scale out stacks\. This section describes the basics of how to scale out a stack by adding a second 24/7 PHP App Server instance to MyStack and putting both instances behind an Elastic Load Balancing load balancer\. You can easily extend the procedure to add an arbitrary number of 24/7 instances, or you can use time\-based or load\-based instances to have AWS OpsWorks Stacks scale your stack automatically\. For more information, see [Managing Load with Time\-based and Load\-based Instances](workinginstances-autoscaling.md)\. 