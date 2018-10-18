# AWS OpsWorks Stacks Layer Reference<a name="layers"></a>

Every instance that AWS OpsWorks Stacks deploys must be a member of at least one layer, which defines an instance's role in the stack and controls the details of setting up and configuring the instance, installing packages, deploying applications, and so on\. For more information about how to use the AWS OpsWorks Stacks to create and manage layers, see [Layers](workinglayers.md)\.

Each layer description includes a list of the built\-in recipes that AWS OpsWorks Stacks runs for each of the layer's lifecycle events\. Those recipes are stored at [https://github\.com/aws/opsworks\-cookbooks](https://github.com/aws/opsworks-cookbooks)\. Note that the lists include only those recipes that are run directly by AWS OpsWorks Stacks\. Those recipes sometimes run dependent recipes, which are not listed\. To see the complete list of recipes for a particular event, including dependent and custom recipes, examine the run list in the appropriate [lifecycle event's Chef log](troubleshoot-debug-log.md)\.

**Topics**
+ [HAProxy Layer Reference](layers-load.md)
+ [HAProxy AWS OpsWorks Stacks Layer](layers-haproxy.md)
+ [MySQL Layer Reference](layers-mysql.md)
+ [MySQL OpsWorks Layer](workinglayers-db-mysql.md)
+ [Application Server Layers Reference](layers-server.md)
+ [Application Server Layers](workinglayers-servers.md)
+ [ECS Cluster Layer Reference](#w4ab1c11c63b7c19c21)
+ [Custom Layer Reference](layers-other-custom.md)
+ [Other Layers Reference](layers-other.md)
+ [Other Layers](workinglayers-other.md)

## ECS Cluster Layer Reference<a name="w4ab1c11c63b7c19c21"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

An ECS Cluster layer represents an [Amazon Elastic Container Service \(Amazon ECS\) cluster](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) and simplifies cluster management\.

**Short name:** ecs\-cluster

**Compatibility:** An [Amazon ECS service](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) layer is compatible only with custom layers

**Open ports:** ECS Cluster allows public access to port 22 \(SSH\)

**Autoassign Elastic IP addresses:** Off by default

**Default EBS volume:** No

**Default security group:** AWS\-OpsWorks\-ECS\-Cluster

**Configuration:** To configure an ECS Cluster layer, you must specify the following:
+ Whether to assign public IP addresses or Elastic IP addresses to the container instances
+ The instance profile for the container instances 

**Setup recipes:**
+  opsworks\_initial\_setup
+ ssh\_host\_keys
+ ssh\_users
+ mysql::client
+ dependencies
+ ebs
+ opsworks\_ganglia::client
+ opsworks\_ecs::setup

**Configure recipes:**
+ opsworks\_ganglia::configure\-client
+ ssh\_users
+ mysql::client
+ agent\_version
+ opsworks\_ecs::configure

**Deploy recipes:**
+ deploy::default
+ opsworks\_ecs::deploy 

**Undeploy recipes:**
+ opsworks\_ecs::undeploy 

**Shutdown recipes:**
+ opsworks\_shutdown::default
+ opsworks\_ecs::shutdown

**Installation:**
+ AWS OpsWorks Stacks uses the instance's package installer to install Docker to its default locations
+ The Chef log for the Setup event notes whether the Amazon ECS agent was successfully installed\. Otherwise, the logs provided by AWS OpsWorks Stacks do not include Amazon ECS error log information\. For more information on how to handleAmazon ECS errors, see [Amazon ECS Troubleshooting](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/troubleshooting.html)\.