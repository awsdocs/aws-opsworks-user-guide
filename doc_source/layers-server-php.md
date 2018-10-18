# PHP App Server Layer Reference<a name="layers-server-php"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The PHP App Server layer supports a PHP application server by using [Apache2](http://httpd.apache.org/) with mod\_php\.

**Short name:** php\-app

**Compatibility:** A PHP App Server layer is compatible with the following layers: custom, db\-master, memcached, monitoring\-master, and rails\-app\.

**Open ports:** A PHP App Server layer allows public access to ports 22 \(SSH\), 80 \(HTTP\), 443 \(HTTPS\), and all ports from load balancers\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS volume:** No

**Default security group:** AWS\-OpsWorks\-PHP\-App\-Server

**Setup recipes:**
+  opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ mysql::client
+ dependencies
+ mod\_php5\_apache2 

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version
+ mod\_php5\_apache2::php
+ php::configure 

**Deploy recipes:**
+ deploy::default
+ deploy::php 

**Undeploy recipes:**
+ deploy::php\-undeploy

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ apache2::stop 

**Installation: **
+  AWS OpsWorks Stacks uses the instance's package installer to install Apache2, mod\_php and the associated log files to their default locations\. For more information about installation, see [Apache](http://httpd.apache.org/)\. For more information about logging, see [Log Files](http://httpd.apache.org/docs/2.2/logs.html)\.