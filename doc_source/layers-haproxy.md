# HAProxy AWS OpsWorks Stacks Layer<a name="layers-haproxy"></a>

**Note**  
This layer is available only for Chef 11 and earlier Linux\-based stacks\.

The AWS OpsWorks Stacks HAProxy layer is an AWS OpsWorks Stacks layer that provides a blueprint for instances that host an [HAProxy](http://haproxy.1wt.eu/) server—a reliable high\-performance TCP/HTTP load balance\. One small instance is usually sufficient to handle all application server traffic\. 

**Note**  
Stacks are limited to a single region\. To distribute your application across multiple regions, you must create a separate stack for each region\.

**To create a HAProxy layer**

1. In the navigation pane, click **Layers**\.

1. On the **Layers** page, click **Add a Layer** or **\+ Layer**\. For **Layer type**, select **HAProxy**\.

The layer has the following configuration settings, all of which are optional\.

**HAProxy statistics**  
Whether the layer collects and displays statistics\. The default value is **Yes**\.

**Statistics URL**  
The statistics page's URL path\. The complete URL is http://*DNSName**StatisticsPath*, where *DNSName* is the associated instance's DNS name\. The default *StatisticsPath* value is /haproxy?stats, which corresponds to something like: http://ec2\-54\-245\-151\-7\.us\-west\-2\.compute\.amazonaws\.com/haproxy?stats\.

**Statistics user name**  
The statistics page's user name, which you must provide to view the statistics page\. The default value is "opsworks"\.

**Statistics password**  
A statistics page password, which you must provide to view the statistics page\. The default value is a randomly generated string\.

**Health check URL**  
The health check URL suffix\. HAProxy uses this URL to periodically call an HTTP method on each application server instance to determine whether the instance is functioning\. If the health check fails, HAProxy stops routing traffic to the instance until it is restarted, either manually or through [auto healing](workinginstances-autohealing.md)\. The default value for the URL suffix is "/", which corresponds to the server instance's home page: http://*DNSName*/\. 

**Health check method**  
An HTTP method to be used to check whether instances are functioning\. The default value is **OPTIONS** and you can also specify **GET** or **HEAD**\. For more information, see [httpchk](http://cbonte.github.io/haproxy-dconv/configuration-1.5.html)\. 

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. Make sure that the group has the correct settings to allow traffic between layers\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add_layer_haproxy.png)

**Note**  
Record the password for later use; AWS OpsWorks Stacks does not allow you to view the password after you create the layer\. However, you can update the password by going to the layer's **Edit** page and clicking **Update password** on the **General Settings** tab\.  

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/haproxy_update_password.png)

## How the HAProxy Layer Works<a name="w4ab1c11c63b7c19c11c19"></a>

By default, HAProxy does the following:
+ Listens for requests on the HTTP and HTTPS ports\.

  You can configure HAProxy to listen on only the HTTP or HTTPS port by overriding the Chef configuration template, `haproxy.cfg.erb`\.
+ Routes incoming traffic to instances that are members of any application server layer\.

  By default, AWS OpsWorks Stacks configures HAProxy to distribute traffic to instances that are members of any application server layer\. You could, for example, have a stack with both Rails App Server and PHP App Server layers, and an HAProxy master distributes traffic to the instances in both layers\. You can configure the default routing by using a custom recipe\.
+ Routes traffic across multiple Availability Zones\.

  If one Availability Zone goes down, the load balancer routes incoming traffic to instances in other zones so your application continues to run without interruption\. For this reason, a recommended practice is to distribute your application servers across multiple Availability Zones\.
+ Periodically runs the specified health check method on each application server instance to assess its health\.

  If the method does not return within a specified timeout period, the instance is presumed to have failed and HAProxy stops routing requests to the instance\. AWS OpsWorks Stacks also provides a way to automatically replace failed instances\. For more information, see [Using Auto Healing](workinginstances-autohealing.md)\. You can change the health check method when you create the layer\. 
+ Collects statistics and optionally displays them on a web page\.

**Important**  
For health check to work correctly with the default OPTIONS method, your app must return a 2xx or 3xx status code\.

By default, when you add an instance to a HAProxy layer, AWS OpsWorks Stacks assigns it an Elastic IP address to represent the application, which is public to the world\. Because the HAProxy instance's Elastic IP address is the application's only publicly exposed URL, you don't have to create and manage public domain names for the underlying application server instances\. You can obtain the address by going to the **Instances** page and examining the instance's public IP address, as the following illustration shows\. An address that is followed by \(EIP\) is an Elastic IP address\. For more information on Elastic IP addresses, see [Elastic IP Addresses \(EIP\)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/load_balancer_elastic_ip.png)

When you stop an HAProxy instance, AWS OpsWorks Stacks retains the Elastic IP address and reassigns it to the instance when you restart it\. If you delete an HAProxy instance, by default, AWS OpsWorks Stacks deletes the instance's IP address\. To retain the address, clear the **Delete instance's Elastic IP** option, as shown in the following illustration\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_lb.png)

This option affects what happens when you add a new instance to the layer to replace a deleted instance:
+ If you retained the deleted instance's Elastic IP address, AWS OpsWorks Stacks assigns the address to the new instance\.
+ Otherwise, AWS OpsWorks Stacks assigns a new Elastic IP address to the instance and you must update your DNS registrar settings to map to the new address\.

When application server instances come on line or go off line—either manually or as a consequence of [automatic scaling](workinginstances-autoscaling.md) or [auto healing](workinginstances-autohealing.md)—the load balancer configuration must be updated to route traffic to the current set of online instances\. This task is handled automatically by the layer's built\-in recipes:
+ When new instances come on line, AWS OpsWorks Stacks triggers a Configure [lifecycle event](workingcookbook-events.md)\. The HAProxy layer's built\-in Configure recipes update the load balancer configuration so that it also distributes requests to any new application server instances\.
+ When instances go off line or an instance fails a health check, AWS OpsWorks Stacks also triggers a Configure lifecycle event\. The HAProxy Configure recipes update the load balancer configuration to route traffic to only the remaining online instances\. 

Finally, you can also use a custom domain with the HAProxy layer\. For more information, see [Using Custom Domains](workingapps-domains.md)\. 

## Statistics Page<a name="w4ab1c11c63b7c19c11c21"></a>

If you have enabled the statistics page, the HAProxy displays a page containing a variety of metrics at the specified URL\.

**To view HAProxy statistics**

1. Obtain the HAProxy instance's **Public DNS** name from the instance's **Details** page and copy it\.

1. On the **Layers** page, click **HAProxy** to open the layer's details page\.

1. Obtain the the statistics URL from the layer details and append it to the Public DNS name\. For example: **http://ec2\-54\-245\-102\-172\.us\-west\-2\.compute\.amazonaws\.com/haproxy?stats**\. to it\.

1. Paste the URL from the previous step into your browser and use the user name and password that you specified when you created the layer to open the statistics page\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/haproxy_stats.png)