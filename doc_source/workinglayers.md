# Layers<a name="workinglayers"></a>

Every stack contains one or more layers, each of which represents a stack component, such as a load balancer or a set of application servers\.

As you work with AWS OpsWorks Stacks layers, keep the following in mind:
+ Each layer in a stack must have at least one instance and can optionally have multiple instances\.
+ Each instance in a stack must be a member of at least one layer, except for [registered instances](registered-instances.md)\.

  You cannot configure an instance directly, except for some basic settings such as the SSH key and hostname\. You must create and configure an appropriate layer, and add the instance to the layer\.

Amazon EC2 instances can optionally be a member of multiple layers\. In that case, AWS OpsWorks Stacks runs the recipes to install and configure packages, deploy applications, and so on for each of the instance's layers\.

By assigning an instance to multiple layers, you could, for example do the following:
+ Reduce expenses by hosting the database server and the load balancer on a single instance\.
+ Use one of your application servers for administration\.

  Create a custom administrative layer and add one of the application server instances to that layer\. The administrative layer's recipes configure that application server instance to perform administrative tasks, and install any additional required software\. The other application server instances are just application servers\.

This section describes how to work with layers\.

**Topics**
+ [OpsWorks Layer Basics](workinglayers-basics.md)
+ [Elastic Load Balancing Layer](layers-elb.md)
+ [Amazon RDS Service Layer](workinglayers-db-rds.md)
+ [ECS Cluster Layers](workinglayers-ecscluster.md)
+ [Custom AWS OpsWorks Stacks Layers](workinglayers-custom.md)
+ [Per\-layer Operating System Package Installations](per-layer-os-package-install.md)