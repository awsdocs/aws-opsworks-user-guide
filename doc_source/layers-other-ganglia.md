# Ganglia Layer Reference<a name="layers-other-ganglia"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

A Ganglia layer supports [Ganglia](http://ganglia.sourceforge.net/), a distributed monitoring system that manages the storage and visualization of instance metrics\. It is designed to work with hierarchical instance topologies, which makes it particularly useful for groups of instances\. Ganglia has two basic components:
+ A low\-overhead client, which is installed on each instance in the stack and sends metrics to the master\.
+ A master, which collects metrics from the clients and stores them on an Amazon EBS volume\. It also displays the metrics on a web page\.

AWS OpsWorks Stacks has a Ganglia monitoring agent on each instance that it manages\. When you add a Ganglia layer to your stack and start it, the Ganglia agents on each instance report metrics to the Ganglia instance\. To use Ganglia, add a Ganglia layer with one instance to the stack\. You access the data by logging in to the Ganglia backend at the master's IP address\. You can provide additional metric definitions by writing Chef recipes\. 

**Short name:** monitoring\-master

**Compatibility:** A Ganglia layer is compatible with the following layers: custom, db\-master, memcached, php\-app, rails\-app\.

**Open ports:** Load\-Balancer allows public access to ports 22\(SSH\), 80 \(HTTP\), and 443 \(HTTPS\)\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS volume:** Yes, at `/vol/ganglia`

**Default security group:** AWS\-OpsWorks\-Monitoring\-Master\-Server

**Configuration:** To configure a Ganglia layer, you must specify the following:
+ The URI that provides access to the monitoring graphs\. The default value is http://*DNSName*/ganglia, where *DNSName* is the Ganglia instance's DNS name\.
+ A user name and password that control access to the monitoring statistics\.

**Setup recipes:**
+ opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ opsworks\_ganglia::server 

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version
+ opsworks\_ganglia::configure\-server 

**Deploy recipes:**
+ deploy::default
+ opsworks\_ganglia::configure\-server
+ opsworks\_ganglia::deploy 

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ apache2::stop 

**Installation:**
+ The Ganglia client is installed under: `/etc/ganglia`\.
+ The Ganglia web front end is installed under: `/usr/share/ganglia-webfrontend`\.
+ The Ganglia logtailer is installed under: `/usr/share/ganglia-logtailer`\.