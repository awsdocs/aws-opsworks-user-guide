# Elastic Load Balancing Layer<a name="layers-elb"></a>

Elastic Load Balancing works somewhat differently than an AWS OpsWorks Stacks layer\. Instead of creating a layer and adding instances to it, you use the Elastic Load Balancing console or API to create a load balancer and then attach it to an existing layer\. In addition to distributing traffic to the layer's instances, Elastic Load Balancing does the following:
+ Detects unhealthy Amazon EC2 instances and reroutes traffic to the remaining healthy instances until the unhealthy instances have been restored\.
+ Automatically scales request handling capacity in response to incoming traffic\.
+ If you enable [connection draining](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/config-conn-drain.html), the load balancer stops sending new requests to instances that are unhealthy or about to be deregistered but keeps the connection alive, up to a specified timeout value, to allow the instance to complete any in\-flight requests\.

After you attach a load balancer to a layer, AWS OpsWorks Stacks does the following:
+ Deregisters any currently registered instances\.
+ Automatically registers the layer's instances when they come online and deregisters instances when they go offline, including load\-based and time\-based instances\.
+ Automatically starts routing requests to registered instances in their Availability Zones\.

If you have enabled the load balancer's [connection draining](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/config-conn-drain.html) feature, you can specify whether AWS OpsWorks Stacks supports it\. If you enable connection draining support \(the default setting\), after an instance is shut down, AWS OpsWorks Stacks does the following: 
+ Deregisters the instance from the load balancer\.

  The load balancer stops sending new requests and starts connection draining\.
+ Delays triggering a [Shutdown lifecycle event](workingcookbook-events.md) until the load balancer has completed connection draining\.

If you don't enable connection draining support, AWS OpsWorks Stacks triggers the Shutdown event as soon as the instance is shut down, even if the instance is still connected to the load balancer\. 

To use Elastic Load Balancing with a stack, you must first create one or more load balancers in the same region by using the Elastic Load Balancing console, CLI, or API\. You should be aware of the following: 
+ You can attach only one load balancer to a layer\.
+ Each load balancer can handle only one layer\.
+ AWS OpsWorks Stacks does not support Application Load Balancer\. You can only use Classic Load Balancer with AWS OpsWorks Stacks\.

This means that you must create a separate Elastic Load Balancing load balancer for each layer in each stack that you want to balance and use it only for that purpose\. A recommended practice is to assign a distinctive name to each Elastic Load Balancing load balancer that you plan to use with AWS OpsWorks Stacks, such as MyStack1\-RailsLayer\-ELB, to avoid using a load balancer for more than one purpose\. 

**Important**  
We recommend creating new Elastic Load Balancing load balancers for your AWS OpsWorks Stacks layers\. If you choose to use an existing Elastic Load Balancing load balancer, you should first confirm that it is not being used for other purposes and has no attached instances\. After the load balancer is attached to the layer, OpsWorks removes any existing instances and configures the load balancer to handle only the layer's instances\. Although it is technically possible to use the Elastic Load Balancing console or API to modify a load balancer's configuration after attaching it to a layer, you should not do so; the changes will not be permanent\. 

**To attach an Elastic Load Balancing load balancer to a layer**

1. If you have not yet done so, use the [Elastic Load Balancing console](https://console.aws.amazon.com/ec2/#s=LoadBalancers), API, or CLI to create a load balancer in the stack's region\. When you create the load balancer, do the following:
   + Be sure to specify a health check ping path that is appropriate for your application\.

     The default ping path is `/index.html`, so if your application root does not include `index.html`, you must specify an appropriate ping path or the health check will fail\.
   + If you want to use [connection draining](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/config-conn-drain.html), ensure that the feature is enabled and has an appropriate timeout value\.

   For more information, see [Elastic Load Balancing](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/Welcome.html)\.

1. [Create the layer](workinglayers-basics-create.md) that you want to have balanced or [edit an existing layer's Network settings](workinglayers-basics-edit.md)\.
**Note**  
You cannot attach a load balancer when you create a custom layer\. You must edit the layer's settings\.

1. Under **Elastic Load Balancing**, select the load balancer that you want to attach to the layer and specify whether you want AWS OpsWorks Stacks to support connection draining\.

After you attach a load balancer to a layer, AWS OpsWorks Stacks triggers a [Configure lifecycle event](workingcookbook-events.md) on the stack's instances to notify them of the change\. AWS OpsWorks Stacks also triggers a Configure event when you detach a load balancer\.

**Note**  
After an instance has booted, AWS OpsWorks Stacks runs the [Setup and Deploy recipes](workingcookbook-executing.md), which install packages and deploy applications\. After those recipes have finished, the instance is in the online state and AWS OpsWorks Stacks registers the instance with Elastic Load Balancing\. AWS OpsWorks Stacks also triggers a Configure event after the instance comes online\. This means that Elastic Load Balancing registration and the Configure recipes could run concurrently, and the instance might be registered before the Configure recipes have finished\. To ensure that a recipe finishes before an instance is registered with Elastic Load Balancing, you should add the recipe to the layer's Setup or Deploy lifecycle events\. For more information, see [Executing Recipes](workingcookbook-executing.md)\.

It is sometimes useful to remove an instance from a load balancer\. For example, when you update an app, we recommend that you deploy the app to a single instance and verify that the app is working properly before deploying it to every instance\. You typically remove that instance from the load balancer, so it does not receive user requests until you have verified the update\.

You must use the Elastic Load Balancing console or API to temporarily remove an online instance from a load balancer\. The following describes how to use the console\.

**To temporarily remove an instance from a load balancer**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/) and choose **Load Balancers**\.

1. Choose the appropriate load balancer and open the **Instances** tab\.

1. Choose **Remove from Load Balancer** in the instance's **Actions** column\.

1. When you have finished, choose **Edit Instances**, and return the instance to the load balancer\.

**Important**  
If you use the Elastic Load Balancing console or API to remove an instance from a load balancer, you must also use Elastic Load Balancing to put it back\. AWS OpsWorks Stacks is not aware of operations that you perform with other service consoles or APIs, and it will not return the instance to the load balancer for you\. Sometimes, AWS OpsWorks Stacks can add the instance back to the ELB, but this is not guaranteed behavior and does not occur in all cases\.

You can attach multiple load balancers to a particular set of instances as follows:

**To attach multiple load balancers**

1. Use the [Elastic Load Balancing console](https://console.aws.amazon.com/ec2/#s=LoadBalancers), API, or CLI to create a set of load balancers\.

1. [Create a custom layer](workinglayers-custom.md) for each load balancer and attach one of the load balancers to it\. You don't need to implement any custom recipes for these layers; a default custom layer is sufficient\.

1. [Add the set of instances](workinginstances-add.md) to each custom layer\. 

You can examine a load balancer's properties by going to the Instances page and clicking the appropriate load balancer name\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elb_view.png)

The **ELB** page shows the load balancer's basic properties, including its DNS name and the health status of the associated instances\. If the stack is running in a VPC, the page shows subnets rather than Availibility Zones\. A green check indicates a healthy instance\. You can click on the name to connect to a server, through the load balancer\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elb_properties.png)