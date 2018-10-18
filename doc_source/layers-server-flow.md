# AWS Flow \(Ruby\) Layer Reference<a name="layers-server-flow"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

An AWS Flow \(Ruby\) layer provides a blueprint for instances that host Amazon Simple Workflow Service activity and workflow workers\.

**Short name:** aws\-flow\-ruby

**Compatibility:** An AWS Flow \(Ruby\) layer is compatible with PHP App Server, MySQL, Memcached, Ganglia, and custom layers\.

**Open ports:** None\.

**IAM role:** aws\-opsworks\-ec2\-role\-with\-swf is the standard AWS Flow \(Ruby\) role that AWS OpsWorks Stacks creates for you, if requested\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS Volume:** No

**Default security group:** AWS\-OpsWorks\-AWS\-Flow\-Ruby\-Server

**Setup recipes:**
+ opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ opsworks\_aws\_flow\_ruby::setup

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ mysql::client
+ agent\_version
+ opsworks\_aws\_flow\_ruby::configure 

**Deploy recipes:**
+ deploy::default
+ deploy::aws\-flow\-ruby 

**Undeploy recipes:**
+ deploy::aws\-flow\-ruby\-undeploy

**Shutdown recipes:**
+ opsworks\_shutdown::default