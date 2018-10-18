# Load Balancing a Layer<a name="best-server-load-balancing"></a>

AWS OpsWorks Stacks provides two load balancing options, [Elastic Load Balancing](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/elastic-load-balancing.html) and [HAProxy](http://www.haproxy.org/), which are typically used to balance load across an application server layer's instances\. This topic describes the benefits and limitations of each to help you decide which option to choose when adding load balancing to a layer\. In some cases, the best approach is to use both\.

**SSL Termination**  <a name="best-server-load-balancing-ssl"></a>
The built\-in HAProxy layer does not handle SSL termination; you must terminate SSL at the servers\. The advantage of this approach is that traffic is encrypted until it reaches the servers\. However, the servers must handle decryption, which increases server load\. In addition, you must put your SSL certificates on the application servers, which are more accessible to users\.  
With Elastic Load Balancing, you can terminate SSL at the load balancer\. This reduces the load on your application servers, but traffic between the load balancer and the server is not encrypted\. Elastic Load Balancing also allows you [to terminate SSL at the server](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/elb-https-load-balancers.html), but it is somewhat complicated to set up\.

**Scaling**  <a name="best-server-load-balancing-scaling"></a>
If incoming traffic exceeds the capacity of an HAProxy load balancer, you must increase its capacity manually\.   
Elastic Load Balancing automatically scales to handle incoming traffic\. To ensure that an Elastic Load Balancing load balancer has sufficient capacity to handle the expected load when it first comes online, you can [pre\-warm](https://aws.amazon.com/articles/1636185810492479#pre-warming) it\.

**Load Balancer Failure**  <a name="best-server-load-balancing-failure"></a>
If the instance hosting your HAProxy server fails, it could take your entire site offline until you can restart the instance\.  
Elastic Load Balancing is more failure resistant than HAProxy\. For example, it provisions load balancing nodes in each Availability Zone that has registered EC2 instances\. If service in one zone is disrupted, the other nodes continue to handle incoming traffic\. For more information, see [Elastic Load Balancing Concepts](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/TerminologyandKeyConcepts.html)\.

**Idle Timeout**  <a name="best-server-load-balancing-timeout"></a>
Both load balancers terminate a connection if a server is idle for more than a specified idle timeout value\.   
+ HAProxy – The idle timeout value does not have an upper limit\.
+ Elastic Load Balancing – The default idle timeout value is 60 seconds, with a maximum of 3600 seconds \(60 minutes\)\.
The Elastic Load Balancing idle time limit is sufficient for most purposes\. We recommend using HAProxy if you require a longer idle timeout\. For example:  
+ A long\-running HTTP connection that is used for push notifications\.
+ An administrative interface that you use to perform tasks that could take longer than 60 minutes\. 

**URL\-based Mapping**  <a name="best-server-load-balancing-url"></a>
You might want to have a load balancer forward an incoming request to a particular server based on the request's URL\. For example, suppose you have a group of ten application servers that supports an online commerce application\. Eight of the servers handle the catalog and two handle payments\. You want to direct all payment\-related HTTP requests to the payment servers, based on the request URL\. In this case, you would direct all URLs that include "payment" or "checkout" to one of the payment servers\.  
With HAProxy, you can use URL\-based mapping to direct URLs containing a specified string to particular servers\. To use URL\-based mapping with AWS OpsWorks Stacks, you must create a custom HAProxy configuration file by overriding the `haproxy-default.erb` template in the `haproxy` built\-in cookbook\. For more information, see [HAProxy Configuration Manual](http://cbonte.github.io/haproxy-dconv/configuration-1.5.html) and [Using Custom Templates](workingcookbook-template-override.md)\. You cannot use URL\-based mapping for HTTPS requests\. An HTTPS request is encrypted, so HAProxy has no way to examine the request URL\.  
Elastic Load Balancing has limited support for URL mapping\. For more information, see [Listener Configurations for Elastic Load Balancing](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/elb-listener-config.html)\.

**Recommendation:** We recommend using Elastic Load Balancing for load balancing unless you have requirements that can be handled only by HAProxy\. In that case, the best approach might be combining the two by using Elastic Load Balancing as a front\-end load balancer that distributes incoming traffic to a set of HAProxy servers\. To do this:
+ Set up an HAProxy instance in each of your stack's Availability Zones to distribute requests to the zone's application servers\.
+ Assign the HAProxy instances to an Elastic Load Balancing load balancer, which then distributes incoming requests to the HAProxy load balancers\.

This approach allows you to use HAProxy's URL\-based mapping to distribute different types of requests to the appropriate application servers\. However, if one of the HAProxy servers goes offline, the site will continue to function because the Elastic Load Balancing load balancer automatically distributes incoming traffic to the healthy HAProxy servers\. Note that you must use Elastic Load Balancing as the front\-end load balancer; an HAProxy server cannot distribute requests to other HAProxy servers\.