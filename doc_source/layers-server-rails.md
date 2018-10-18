# Rails App Server Layer Reference<a name="layers-server-rails"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The Rails App Server layer supports a [Ruby on Rails](http://rubyonrails.org/) application server\.

**Short name:** rails\-app

**Compatibility:** A Rails App Server layer is compatible with the following layers: custom, db\-master, memcached, monitoring\-master, php\-app\.

**Ports:** A Rails App Server layer allows public access to ports 22\(SSH\), 80 \(HTTP\), 443 \(HTTPS\), and all ports from load balancers\.

**Autoassign Elastic IP addresses:** Off by default

**Default EBS volume:** No

**Default security group:** AWS\-OpsWorks\-Rails\-App\-Server

**Configuration:** To configure a Rails App Server layer, you must specify the following:
+ Ruby version
+ Rails stack
+ Rubygems version
+ Whether to install and manage [Bundler](http://gembundler.com/)
+ The Bundler version

**Setup recipes:**
+ opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ apache2 apache2::mod\_deflate
+ passenger\_apache2
+ passenger\_apache2::mod\_rails
+ passenger\_apache2::rails 

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ agent\_version
+ rails::configure 

**Deploy recipes:**
+ deploy::default
+ deploy::rails

**Undeploy recipes:**
+ deploy::rails\-undeploy 

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ apache2::stop 

**Installation:**
+ AWS OpsWorks Stacks uses the instance's package installer to install Apache2 with mod\_passenger, mod\_rails, and the associated log files to their default locations\. For more information about installation, see [Phusion Passenger](https://www.phusionpassenger.com/)\. For more information about logging, see [Log Files](http://httpd.apache.org/docs/2.2/logs.html)\.