# Node\.js App Server Layer Reference<a name="layers-server-nodejs"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

A Node\.js App Server layer supports a [Node\.js](http://nodejs.org/) application server, which is a platform for implementing highly scalable network application servers\. Programs are written in JavaScript, using event\-driven asynchronous I/O to minimize overhead and maximize scalability\.

**Short name:** nodejs\-app

**Compatibility:** A Node\.js App Server layer is compatible with the following layers: custom, db\-master, memcached, and monitoring\-master\.

**Open ports:** A Node\.js App Server layer allows public access to ports 22 \(SSH\), 80 \(HTTP\), 443 \(HTTPS\), and all ports from load balancers\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS volume:** No

**Default security group:** AWS\-OpsWorks\-nodejs\-App\-Server

**Setup recipes:**
+  opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ opsworks\_nodejs
+ opsworks\_nodejs::npm 

**Configure recipes:**
+  opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version
+ opsworks\_nodejs::configure 

**Deploy recipes:**
+ deploy::default
+ opsworks\_nodejs
+ opsworks\_nodejs::npm
+ deploy::nodejs 

**Undeploy recipes:**
+ deploy::nodejs\-undeploy

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ deploy::nodejs\-stop

**Installation:**
+ Node\.js installs to `/usr/local/bin/node`\.
+ For more information about how to produce log files, see [How to log in node\.js](https://docs.nodejitsu.com/articles/intermediate/how-to-log/) on the Nodejitsu website\.

**Node\.js application configuration:**
+ The main file run by Node\.js must be named `server.js` and reside in the root directory of the deployed application\.
+ The Node\.js application must be set to listen on port 80 \(or port 443, if applicable\)\.

**Note**  
Node\.js apps that run Express commonly use the following code to set the listening port, where `process.env.PORT` represents the default port and resolves to 80:  

```
app.set('port', process.env.PORT || 3000);
```
With AWS OpsWorks Stacks, you must explicitly specify port 80, as follows:  

```
app.set('port', 80);
```