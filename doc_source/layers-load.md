# HAProxy Layer Reference<a name="layers-load"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

A HAProxy layer uses [HAProxy](http://haproxy.1wt.eu/)—a reliable high\-performance TCP/HTTP load balancer— to provide high availability load balancing and proxy services for TCP\- and HTTP\-based applications\. It is particularly useful for websites that must crawl under very high loads while requiring persistence or Layer 7 processing\.

HAProxy monitors traffic and displays the statistics and the health of the associated instances on a web page\. By default, the URI is http://*DNSName*/haproxy?stats, where *DNSName* is the HAProxy instance's DNS name\.

**Short name:** lb

**Compatibility:** A HAProxy layer is compatible with the following layers: custom, db\-master, and memcached\.

**Open ports:** HAProxy allows public access to ports 22 \(SSH\), 80 \(HTTP\), and 443 \(HTTPS\)\.

**Autoassign Elastic IP addresses:** On by default

**Default EBS volume:** No

**Default security group:** AWS\-OpsWorks\-LB\-Server

**Configuration:** To configure a HAProxy layer, you must specify the following:
+ Health check URI \(default: http://*DNSName*/\)\.
+ Statistics URI \(default: http://*DNSName*/haproxy?stats\)\.
+ Statistics password \(optional\)\.
+ Health check method \(optional\)\. By default, HAProxy uses the HTTP OPTIONS method\. You can also specify GET or HEAD\.
+ Enable statistics \(optional\)
+ Ports\. By default, AWS OpsWorks Stacks configures HAProxy to handle both HTTP and HTTPS traffic\. You can configure HAProxy to handle only one or the other by overriding the Chef configuration [template](https://github.com/aws/opsworks-cookbooks/tree/master-chef-11.4/haproxy/templates/default), `haproxy.cfg.erb`\.

**Setup recipes:**
+  opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ haproxy

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version
+ haproxy::configure

**Deploy recipes:**
+ deploy::default
+ haproxy::configure 

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ haproxy::stop

**Installation:**
+ AWS OpsWorks Stacks uses the instance's package installer to install HAProxy to its default locations\.
+ You must set up syslog to direct the log files to a specified location\. For more information, see [HAProxy](http://haproxy.1wt.eu/)\.