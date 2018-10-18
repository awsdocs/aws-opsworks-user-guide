# Best Practices: Managing and Deploying Apps and Cookbooks<a name="best-deploy"></a>

AWS OpsWorks Stacks deploys apps and cookbooks to each new instance from a remote repository\. During an instance's lifetime, you often must update the apps or cookbooks on the stack's online instances to add features, fix bugs, and so on\. There are a variety of ways to manage a stack's apps and cookbooks, but the approach you use should satisfy the following general requirements:
+ All production stack instances should have the same application and custom cookbook code, with limited exceptions for purposes such as A/B testing\.
+ Deploying an update should not interrupt the site's operation, even if something goes wrong\. 

This section describes recommended practices for managing and deploying apps and custom cookbooks\.

**Topics**
+ [Maintaining Consistency](#best-deploy-consistency)
+ [Deploying Code to Online Instances](#best-deploy-deploy)

## Maintaining Consistency<a name="best-deploy-consistency"></a>

In general, you need to maintain tight control over the app or cookbook code that runs on your production stack\. Typically, all instances should run the currently approved version of the code\. Exceptions occur when updating your apps or cookbooks, as described later, and when accommodating special cases, such as performing A/B testing\.

App and cookbook code is deployed from a specified source repository to your stack's instances in two ways:
+ When you start an instance, AWS OpsWorks Stacks automatically deploys the current app and cookbook code to the instance\.
+ For online instances, you must manually deploy the current app or cookbook code by running a [Deploy command](workingapps-deploying.md) \(for apps\) or an [Update Custom Cookbooks command](workingstacks-commands.md) \(for cookbooks\)\.

Because there are two deployment mechanisms, it's critical that you manage your source code carefully to avoid unintentionally running different code on different instances\. For example, if you deploy apps or cookbooks from a Git master branch, AWS OpsWorks Stacks deploys what is in that branch at the time\. If you update the code in the master branch and then start a new instance, that instance will have a more recent version of the code than older instances\. The more recent version might not even be approved for production\.

**Recommendation: Amazon S3 Archives**  
To ensure that all your instances have the approved code version, we recommend deploying your apps and cookbooks from an Amazon Simple Storage Service \(Amazon S3\) archive\. This guarantees that the code is a static artifact—a \.zip or other archive file—that must be explicitly updated\. In addition, Amazon S3 is highly reliable, so you will rarely, if ever, be unable to access the archive\. To further ensure consistency, explicitly version each archive file by using a naming convention or by using [Amazon S3 versioning](http://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html), which provides an audit trail and an easy way to revert to an earlier version\.  
For example, you could create a deployment pipeline using a tool such as [Jenkins](https://jenkins.io/index.html)\. After the code that you want to deploy has been committed and tested, create an archive file and upload it to Amazon S3\. All app deployments or cookbook updates will install the code in that archive file and every instance will have the same code\.

**Recommendation: Git or Subversion Repositories**  
If you prefer to use a Git or Subversion repository, don't deploy from the master branch\. Instead, tag the approved version and specify that version as the [app](workingapps-creating.md#workingapps-creating-source) or [cookbook](workingcookbook-installingcustom-enable.md#workingcookbook-installingcustom-enable-repo) source\. 

## Deploying Code to Online Instances<a name="best-deploy-deploy"></a>

AWS OpsWorks Stacks does not automatically deploy updated code to online instances\. You must perform that operation manually, which poses the following challenges:
+ Deploying the update efficiently without compromising the site's ability to handle customer requests during the deployment process\.
+ Handling an unsuccessful deployment, either because of problems with the deployed app or cookbooks or problems with the deployment process itself\.

The simplest approach is to run a default [Deploy command](workingapps-deploying.md) \(for apps\) or [Update Custom Cookbooks command](workingstacks-commands.md) \(for cookbooks\), which deploys the update to every instance concurrently\. This approach is simple and fast, but there is no margin for error\. If the deployment fails or the updated code has any issues, every instance in your production stack could be affected, potentially disrupting or disabling your site until you can fix the problem or roll back to the previous version\.

**Recommendation:** Use a robust deployment strategy, which allows instances running the old version of code to continue handling requests until you have verified that deployment was successful and can confidently transfer all incoming traffic to the new version\. 

The following sections provide two examples of robust deployment strategies, followed by a discussion of how to manage a backend database during deployment\. For brevity, they describe app updates, but you can use similar strategies for cookbooks\.

**Topics**
+ [Using a Rolling Deployment](#best-deploy-rolling)
+ [Using Separate Stacks](#best-deploy-environments)
+ [Managing a Backend Database](#best-deploy-db)

### Using a Rolling Deployment<a name="best-deploy-rolling"></a>

A rolling deployment updates an application on a stack's online application server instances in multiple phases\. With each phase, you update a subset of the online instances and verify that the update is successful before starting the next phase\. If you encounter problems, the instances that are still running the old app version can continue to handle incoming traffic until you resolve the issues\.

The following example assumes that you are using the recommended practice of distributing your stack's application server instances across multiple Availability Zones\. 

**To perform a rolling deployment**

1. On the [Deploy App page](workingapps-deploying.md), choose **Advanced**, choose a single application server instance, and deploy the app to that instance\.

   If you want to be cautious, you can remove the instance from the load balancer before deploying the app\. This ensures that users won't encounter the updated application until you have verified that it is working correctly\. If you use Elastic Load Balancing, [remove the instance](http://docs.aws.amazon.com/opsworks/latest/userguide/load-balancer-elb.html) from the load balancer by using the Elastic Load Balancing console, CLI, or an SDK\. 

1. Verify that the updated app is working correctly and that the instance has acceptable performance metrics\. 

   If you removed the instance from an Elastic Load Balancing load balancer, use the Elastic Load Balancing console, CLI, or an SDK to restore it\. The updated app version is now handling user requests\.

1. Deploy the update to the remainder of the instances in the Availability Zone and verify that they are working correctly and have acceptable metrics\.

1. Repeat step 3 for the stack's other Availability Zones, one zone at a time\. If you want to be especially cautious, repeat steps 1 – 3\.

**Note**  
If you use an Elastic Load Balancing load balancer, you can use its health check to verify that the deployment was successful\. However, set the [ping path](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) to an application that checks dependencies and verifies that everything is working correctly, not a static file that simply confirms that the application server is running\.

### Using Separate Stacks<a name="best-deploy-environments"></a>

Another approach to managing applications is to use a separate stack for each phase of the application's lifecycle\. The different stacks are sometimes referred to as environments\. This arrangement allows you to do development and testing on stacks that are not publicly accessible\. When you are ready to deploy an update, switch user traffic from the stack that hosts the current application version to the stack that hosts the updated version\.

**Topics**
+ [Using Development, Staging, and Production Stacks](#best-deploy-environments-stacks)
+ [Using a Blue\-Green Deployment Strategy](#best-deploy-environments-blue-green)

#### Using Development, Staging, and Production Stacks<a name="best-deploy-environments-stacks"></a>

The most common approach uses the following stacks\.

**Development Stack**  
Use a development stack for tasks such as implementing new features or fixing bugs\. A development stack is essentially a prototype production stack, with the same layers, apps, resources, and so on that are included on your production stack\. Because the development stack usually does not have to handle the same load as the production stack, you typically can use fewer or smaller instances\.   
Development stacks are not public facing; you control access as follows:  
+ Restrict network access by configuring the application server's or load balancer's [security group inbound rules](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) to accept incoming requests only from specified IP addresses or address ranges\.

  For example, limit HTTP, HTTPS, and SSH access to addresses in your corporate address range\.
+ Control access to AWS OpsWorks Stacks stack management functionality by using the stack's [Permissions page](opsworks-security-users.md)\.

  For example, grant a Manage permissions level to the development team, and Show permissions to all other employees\.

**Staging Stack**  
Use a staging stack to test and finalize candidates for an updated production stack\. When you have completed development, create a staging stack by [cloning the development stack](workingstacks-cloning.md)\. Then run your test suite on the staging stack and deploy updates to that stack to fix issues that arise\.  
Staging stacks also are not public facing; you control stack and network access the same way you do for the development stack\. Note that when you clone a development stack to create a staging stack, you can clone the permissions granted by AWS OpsWorks Stacks permissions management\. However, cloning does not affect permissions granted by users' IAM policies\. You must use the IAM console, CLI, or an SDK to modify those permissions\. For more information, see [Managing User Permissions](opsworks-security-users.md)\.

**Production Stack**  
The production stack is the public\-facing stack that supports your current application\. When the staging stack has passed testing, you promote it to production and retire the old production stack\. For an example of how to do this, see [Using a Blue\-Green Deployment Strategy](#best-deploy-environments-blue-green)\.

**Note**  
Instead of using the AWS OpsWorks Stacks console to create stacks manually, create an AWS CloudFormation template for each stack\. This approach has the following advantages:  
Speed and convenience – When you launch the template, AWS CloudFormation automatically creates the stack, including all the required instances\.
Consistency – Store the template for each stack in your source repository to ensure that developers use the same stack for the same purpose\.

#### Using a Blue\-Green Deployment Strategy<a name="best-deploy-environments-blue-green"></a>

A *blue\-green* deployment strategy is one common way to efficiently use separate stacks to deploy an application update to production\.
+ The blue environment is the production stack, which hosts the current application\.
+ The green environment is the staging stack, which hosts the updated application\.

When you are ready to deploy the updated app to production, you switch user traffic from the blue stack to the green stack, which becomes the new production stack\. You then retire the old blue stack\.

The following example describes how to perform a blue\-green deployment with AWS OpsWorks Stacks stacks, in conjunction with [Route 53](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) and a pool of [Elastic Load Balancing load balancers](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/SvcIntro.html)\. Prior to making the switch, you should ensure the following:
+ The application update on the green stack has passed testing and is ready for production\.
+ The green stack is identical to the blue stack except that it includes the updated app and is not public facing\.

  Both stacks have the same permissions, the same number and type of instances in each layer, the same [time\-based and load\-based](workinginstances-autoscaling.md) configuration, and so on\.
+ All of the green stack's 24/7 instances and scheduled time\-based instances are online\.
+ You have a pool of Elastic Load Balancing load balancers that can be dynamically attached to a layer in either stack and can be [pre\-warmed](https://aws.amazon.com/articles/1636185810492479#pre-warming) to handle the expected traffic volume\.
+ You have used the Route 53 [weighted routing feature](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html) to create a record set in a hosted zone that includes your pooled load balancers\.
+ You have assigned a nonzero weight to the load balancer that is attached to your blue stack's application server layer and zero weight to the unused load balancers\. This ensures that the blue stack's load balancer handles all incoming traffic\.

**To switch users to the green stack**

1. [Attach one of the pool's unused load balancers](layers-elb.md) to the green stack's application server layer\. In some scenarios, such as when you expect flash traffic, or if you cannot configure a load test to gradually increase traffic, [pre\-warm](https://aws.amazon.com/articles/1636185810492479#pre-warming) the load balancer to handle the expected traffic\.

1. After all of the green stack's instances have passed the Elastic Load Balancing health check, change the weights in the Route 53 record set so that the green stack's load balancer has a nonzero weight and the blue stack's load balancer has a correspondingly reduced weight\. We recommend that you start by having the green stack handle a small percentage of requests, perhaps 5%, with the blue stack handling the rest\. You now have two production stacks, with the green stack handling some of the incoming requests and the blue stack handling the remainder\. 

1. Monitor the green stack's performance metrics\. If they are acceptable, increase the green stack's weight so that it handles perhaps 10% of the incoming traffic\.

1. Repeat Step 3 until the green stack is handling approximately half of the incoming traffic\. Any issues should have surfaced by this point, so if the green stack is performing acceptably, you can complete the process by reducing the blue stack's weight to zero\. The green stack is now the new blue stack and is handling all incoming traffic\.

1. [Detach the load balancer](layers-elb.md) from the old blue stack's application server layer and return it to the pool\. 

1. Although the old blue stack is no longer handling user requests, we recommend retaining it for a while in case there are problems with the new blue stack\. In that case, you can roll back the update by reversing the procedure to direct incoming traffic back to the old blue stack\. When you are confident that the new blue stack is operating acceptably, [shut down the old blue stack](workingstacks-shutting.md)\.

### Managing a Backend Database<a name="best-deploy-db"></a>

If your application depends on a backend database, you will need to transition from the old application to the new\. AWS OpsWorks Stacks supports the following database options\.

**Amazon RDS Layer**  
With an [Amazon Relational Database Service \(Amazon RDS\) layer](workinglayers-db-rds.md), you create the RDS DB instance separately and then register it with your stack\. You can register an RDS DB instance with only one stack at a time, but you can switch an RDS DB instance from one stack to another\. 

AWS OpsWorks Stacks installs a file with the connection data on your application servers in a format that easily can be used by your application\. AWS OpsWorks Stacks also adds the database connection information to the stack configuration and deployment attributes, which can be accessed by recipes\. You also can use JSON to provide connection data to applications\. For more information, see [Connecting to a Database](workingapps-connectdb.md)\.

Updating an application that depends on a database poses two basic challenges:
+ Ensuring that every transaction is properly recorded during the transition while also avoiding race conditions between the new and old application versions\.
+ Performing the transition in a way that limits the impact on your site's performance and minimizes or eliminates downtime\. 

When you use the deployment strategies described in this topic, you can't simply detach the database from the old application and reattach it to the new one\. Both versions of the application run in parallel during the transition and must have access to the same data\. The following describes two approaches to managing the transition, both of which have advantages and challenges\.

Approach 1: Have both applications connect to the same database     
**Advantages**  
+ There is no downtime during the transition\.

  One application gradually stops accessing the database while the other gradually takes over\.
+ You don't have to synchronize data between two databases\.  
**Challenges**  
+ Both applications access the same database, so you must manage access to prevent data loss or corruption\.
+ If you need to migrate to a new database schema, the old application version must be able to use the new schema\.
If you are using separate stacks, this approach is probably best suited to Amazon RDS because the instance is not permanently tied to a particular stack and can be accessed by applications running on different stacks\. However, you can't register an RDS DB instance with more than one stack at a time, so you must provide connection data to both applications, for example by using JSON\. For more information, see [Using a Custom Recipe](workingapps-connectdb.md#workingapps-connectdb-custom)\.   
If you use a rolling upgrade, the old and new application versions are hosted on the same stack, so you can use either an Amazon RDS or MySQL layer\.

Approach 2: Provide each application version with its own database    
**Advantages**  
+ Each version has its own database, so the schemas don't have to be compatible\.  
**Challenges**  
+ Synchronizing the data between the two databases during the transition without losing or corrupting data\.
+ Ensuring that your synchronization procedure doesn't cause significant downtime or significantly degrade the site's performance\.
If you are using separate stacks, each one has its own database\. If you are using a rolling deployment, you can attach two databases to the stack, one for each application\. If the old and updated applications do not have compatible database schemas, this approach is better\. 

**Recommendation:** In general, we recommend using an Amazon RDS layer as an application's backend database because it is more flexible and can be used for any transition scenario\. For more information about how to handle transitions, see the [Amazon RDS User Guide](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)\.