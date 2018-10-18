# Application Server Layers<a name="workinglayers-servers"></a>

**Note**  
These layers are available only for Chef 11 and earlier Linux\-based stacks\.

AWS OpsWorks Stacks supports several different application servers, where "application" includes static web pages\. Each type of server has a separate AWS OpsWorks Stacks layer, with built\-in recipes that handle installing the application server and any related packages on each of the layer's instances, deploying apps, and so on\. For example, the Java App Server layer installs several packages—including Apache, Tomcat, and OpenJDK—and deploys Java apps to each of the layer's instances\.

The following is the basic procedure for using an application server layers:

1. [Create](workinglayers-basics-create.md) one of the available **App Server** layer types\.

1. [Add one or more instances](workinginstances-add.md) to the layer\.

1. Create apps and deploy them to the instances\. For more information, see [Apps](workingapps.md)\.

1. \(Optional\) If the layer has multiple instances, you can add a load balancer, which distributes incoming traffic across the instances\. For more information, see [HAProxy AWS OpsWorks Stacks Layer](layers-haproxy.md)\.

**Topics**
+ [AWS Flow \(Ruby\) Layer](workinglayers-awsflow.md)
+ [Java App Server AWS OpsWorks Stacks Layer](layers-java.md)
+ [Node\.js App Server AWS OpsWorks Stacks Layer](workinglayers-node.md)
+ [PHP App Server AWS OpsWorks Stacks Layer](workinglayers-php.md)
+ [Rails App Server AWS OpsWorks Stacks Layer](workinglayers-rails.md)
+ [Static Web Server AWS OpsWorks Stacks Layer](workinglayers-static.md)