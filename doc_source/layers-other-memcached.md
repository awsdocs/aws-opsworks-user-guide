# Memcached Layer Reference<a name="layers-other-memcached"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

[Memcached](http://memcached.org/) is a distributed memory\-caching system for arbitrary data\. It speed up websites by caching strings and objects as keys and values in RAM to reduce the number of times an external data source must be read\. 

To use Memcached in a stack, create a Memcached layer and add one or more instances, which function as Memcached servers\. The instances automatically install Memcached and the stack's other instances are able to access and use the Memcached servers\. If you use a Rails App Server layer, AWS OpsWorks Stacks automatically places a `memcached.yml` configuration file in the config directory of each instance in the layer\. You can obtain the Memcached server and port number from this file\.

**Short name:** memcached

**Compatibility:** A Memcached layer is compatible with the following layers: custom, db\-master, lb, monitoring\-master, nodejs\-app, php\-app, rails\-app, and web\.

**Open ports:** A Memcached layer allows public access to port 22\(SSH\) and all ports from the stack's web servers, custom servers, and Rails, PHP, and Node\.js application servers\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS volume:** No

**Default security group:** AWS\-OpsWorks\-Memcached\-Server 

To configure a Memcached layer, you must specify the cache size, in MB\.

**Setup recipes:**
+ opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ memcached 

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version 

**Deploy recipes:**
+ deploy::default

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ memcached::stop

**Installation:**
+ AWS OpsWorks Stacks uses the instance's package installer to install Memcached and its log files in their default locations\.