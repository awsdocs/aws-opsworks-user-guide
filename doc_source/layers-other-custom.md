# Custom Layer Reference<a name="layers-other-custom"></a>

If the standard layers don't suit your requirements, you can create a custom layer\. A stack can have multiple custom layers\. By default, the custom layer runs a limited set of standard recipes that support basic functionality\. You then implement the layer's primary functionality by implementing a set of custom Chef recipes for each of the appropriate lifecycle events to set up and configure the layer's software, and so on\. Custom recipes run after the standard AWS OpsWorks Stacks recipes for each event\. 

**Short name:** User\-defined; each custom layer in a stack must have a different short name

**Open ports:** By default, a custom server layer opens public access to ports 22\(SSH\), 80 \(HTTP\), 443 \(HTTPS\), and all ports from the stack's Rails and PHP application server layers

**Autoassign Elastic IP Addresses:** Off by default

**Default EBS volume:** No

**Default Security Group:** AWS\-OpsWorks\-Custom\-Server

**Compatibility:** Custom layers are compatible with the following layers: custom, db\-master, lb, memcached, monitoring\-master, nodejs\-app, php\-app, rails\-app, and web

**Configuration:** To configure a custom layer, you must specify the following:
+ The layer's name
+ The layer's short name, which identifies the layer in Chef recipes and must use only a\-z and numbers

For Linux stacks, the custom layer uses the following recipes\.

**Setup recipes:**
+  opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version 

**Deploy recipes:**
+ deploy::default

**Shutdown recipes:**
+ opsworks\_shutdown::default