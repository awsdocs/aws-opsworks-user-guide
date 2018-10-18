# Java App Server Layer Reference<a name="layers-server-java"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

A Java App Server layer supports an [Apache Tomcat 7\.0](http://tomcat.apache.org/) application server\.

**Short name:** java\-app

**Compatibility:** A Java App Server layer is compatible with the following layers: custom, db\-master, and memcached\.

**Open ports:** A Java App Server layer allows public access to ports 22 \(SSH\), 80 \(HTTP\), 443 \(HTTPS\), and all ports from load balancers\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS Volume:** No

**Default security group:** AWS\-OpsWorks\-Java\-App\-Server

**Setup recipes:**
+ opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ opsworks\_java::setup

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version
+ opsworks\_java::configure 

**Deploy recipes:**
+ deploy::default
+ deploy::java 

**Undeploy recipes:**
+ deploy::java\-undeploy

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ deploy::java\-stop

**Installation:**
+ Tomcat installs to `/usr/share/tomcat7`\.
+ For more information about how to produce log files, see [Logging in Tomcat](http://tomcat.apache.org/tomcat-6.0-doc/logging.html)\.