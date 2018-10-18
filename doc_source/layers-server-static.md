# Static Web Server Layer Reference<a name="layers-server-static"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The Static Web Server layer serves static HTML pages, which can include client\-side code such as JavaScript\. It is based on [Nginx](http://nginx.org/en/), which is an open source HTTP, reverse proxy, and mail proxy server\.

**Short name:** web

**Compatibility:** A Static Web Server layer is compatible with the following layers: custom, db\-master, memcached\.

**Open ports:** A Static Web Server layer allows public access to ports 22\(SSH\), 80 \(HTTP\), 443 \(HTTPS\), and all ports from load balancers\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS volume:** No

**Default security group:** AWS\-OpsWorks\-Web\-Server

**Setup recipes:**
+ opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ nginx 

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version 

**Deploy recipes:**
+ deploy::default
+ deploy::web 

**Undeploy recipes:**
+ deploy::web\-undeploy

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ nginx::stop

**Installation:**
+ Nginx installs to `/usr/sbin/nginx`\.
+ Nginx log files are in `/var/log/nginx`\.