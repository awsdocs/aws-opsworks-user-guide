# Migrating from Chef Server to AWS OpsWorks Stacks<a name="best-practices-server-migrate"></a>

Because AWS OpsWorks Stacks is based on Chef, migrating from Chef Server to AWS OpsWorks Stacks is relatively straightforward\. This topic provides guidelines for modifying Chef Server code to work with AWS OpsWorks Stacks\.

**Note**  
We do not recommend migrating to stacks using Chef versions earlier than 11\.10, which are based on chef\-solo and do not support search or data bags\.

**Topics**
+ [Mapping Roles to Layers](#best-practices-server-migrate-roles)
+ [Using Data Bags](#best-practices-server-migrate-data-bags)
+ [Using Chef Search](#best-practices-server-migrate-search)
+ [Managing Cookbooks and Recipes](#best-practices-server-migrate-cookbooks)
+ [Using Chef Environments](#best-practices-server-migrate-environments)

## Mapping Roles to Layers<a name="best-practices-server-migrate-roles"></a>

Chef Server uses roles to represent and manage instances with the same purpose and configuration, such as a set of instances that each host a Java application server\. An [AWS OpsWorks Stacks layer](workinglayers.md) serves essentially the same purpose as a Chef role\. A layer is a blueprint for creating a set of Amazon Elastic Compute Cloud \(Amazon EC2\) instances with the same configuration, installed packages, application deployment procedure, and so on\. 

AWS OpsWorks Stacks includes a set of [built\-in layers](workinglayers.md) for several types of application server, an HAProxy load balancer, a MySQL database master, and a Ganglia monitoring master\. For example, the built\-in [Java App Server](layers-java.md) layer is a blueprint for creating instances that host a Tomcat server\.

To migrate to AWS OpsWorks Stacks, you need to associate each role with a layer that provides equivalent functionality\. For some roles, you might be able to simply use one of the built\-in layers\. Other roles might require varying degrees of customization\. Start by examining the functionality of the built\-in layers, including the recipes associated with each, to see if one provides at least some of your role's functionality\. For more information about the built\-in layers, see [Layers](workinglayers.md) and [AWS OpsWorks Stacks Layer Reference](layers.md)\. To examine the built\-in recipes, see [the AWS OpsWorks Stacks public GitHub repository](https://github.com/aws/opsworks-cookbooks)\.

How you proceed depends on how closely you can match a layer to each role, as follows\.

**A built\-in layer supports all of the role's functionality**  
You can use the built\-in layer directly, with minor customizations, if necessary\. For example, if a role supports a Tomcat server, the Java App Server layer's recipes might already handle all of the role's tasks, perhaps with some modest customization\. For example, you can make the layer's built\-in recipes use custom Tomcat or Apache configuration settings by overriding the appropriate [attributes](workingcookbook-attributes.md) or [templates](workingcookbook-template-override.md)\. 

**A built\-in layer supports some, but not all, of the role's functionality**  
You might be able to use a built\-in layer by [extending the layer](workingcookbook-extend.md)\. This typically involves implementing custom recipes to support the missing functionality and assigning the recipes to the layer's lifecycle events\. For example, suppose that your role installs a Redis server on the same instances that host a Tomcat server\. You could extend the Java App Server layer to match the role's functionality by implementing a custom recipe to install Redis on the layer's instances and assigning the recipe to the layer's Setup event\.

**No built\-in layer adequately supports the role's functionality**  
Implement a custom layer\. For example, suppose that your role supports a MongoDB database server, which is not supported by any of the built\-in layers\. You can provide that support by implementing recipes to install the required packages, configure the server, and so on, and assign the recipes to a custom layer's lifecycle events\. Typically, you can use at least some of the role's recipes for this purpose\. For more information about how to implement a custom layer, see [Creating a Custom Tomcat Server Layer](create-custom.md)\. 

## Using Data Bags<a name="best-practices-server-migrate-data-bags"></a>

Chef Server allows you to pass user\-defined data to your recipes by using data bags\.
+ You store the data with your cookbooks, and Chef installs it on each instance\. 
+ You can use encrypted data bags for sensitive data such as passwords\.

 AWS OpsWorks Stacks supports data bags; recipes can retrieve the data using exactly the same code as with Chef Server\. However, the support has the following limitations and differences:
+ Data bags are supported only on Chef 11\.10 Linux and later stacks\.

  Windows stacks and Linux stacks running earlier versions of Chef do not support data bags\.
+ You do not store data bags in your cookbook repository\.

  Instead, you use custom JSON to manage your data bag's data\. 
+ AWS OpsWorks Stacks does not support encrypted data bags\.

  If you need to store sensitive data in encrypted form, such as passwords or certificates, we recommend storing it in a private S3 bucket\. You can then create a custom recipe that uses the [Amazon SDK for Ruby](http://aws.amazon.com/documentation/sdk-for-ruby/) to retrieve the data\. For an example, see [Using the SDK for Ruby](cookbooks-101-opsworks-s3.md)\.

For more information, see [Using Data Bags](workingcookbook-chef11-10.md#workingcookbook-chef11-10-databag)\.

## Using Chef Search<a name="best-practices-server-migrate-search"></a>

Chef Server stores stack configuration information, such as IP addresses and role configurations, on the server\. Recipes use Chef search to retrieve this data\. AWS OpsWorks Stacks uses a somewhat different approach\. For example, Chef 11\.10 Linux stacks are based on Chef client local mode, a Chef client option that runs a lightweight version of Chef Server \(often called Chef Zero\) locally on the instance\. Chef Zero supports search against the data stored in the instance's node object\.

Instead of storing stack data on a remote server, AWS OpsWorks Stacks adds a set of [stack configuration and deployment attributes](workingcookbook-json.md) to each instance's node object for every lifecycle event\. These attributes represent a snapshot of the stack configuration\. They use the same syntax as Chef Server and represent most of the data that recipes need to retrieve from the server\.

You often don't need to modify your recipes' search\-dependent code for AWS OpsWorks Stacks\. Because Chef search operates on the node object, which includes the stack configuration and deployment attributes, search queries in AWS OpsWorks Stacks usually work exactly as they do with Chef Server\. 

The primary exception is caused by the fact that the stack configuration and deployment attributes contain only data that AWS OpsWorks Stacks is aware of when it installs the attributes on the instance\. If you create or modify an attribute locally on a particular instance those changes do not propagate back to AWS OpsWorks Stacks and are not incorporated into the stack configuration and deployment attributes that are installed on the other instances\. You can use search to retrieve the attribute value only on that instance\. For more information, see [Using Chef Search](workingcookbook-chef11-10.md#workingcookbook-chef11-10-search)\.

For compatibility with Chef Server, AWS OpsWorks Stacks adds a set of `role` attributes to the node object, each of which contains one of the stack's layer attributes\. If your recipe uses `roles` as a search key, you don't need to change the search code\. The query automatically returns data for the corresponding layer\. For example, the following queries both return the `php-app` layer's attributes\.

```
phpserver = search(:node, "layers:php-app").first
```

```
phpserver = search(:node, "roles:php-app").first
```

## Managing Cookbooks and Recipes<a name="best-practices-server-migrate-cookbooks"></a>

AWS OpsWorks Stacks and Chef Server handle cookbooks and recipes somewhat differently\. With Chef Server:
+ You provide all of the cookbooks, either by implementing them yourself or by using community cookbooks\.
+ You store cookbooks on the server\.
+ You execute recipes manually or on a regular schedule\.

With AWS OpsWorks Stacks:
+ AWS OpsWorks Stacks provides one or more cookbooks for each of the built\-in layers\. These cookbooks handle standard tasks, such as installing and configuring a built\-in layer's software and deploying apps\.

  To handle tasks that aren't performed by the built\-in cookbooks, you add custom cookbooks to your stack or use community cookbooks\.
+ You store AWS OpsWorks Stacks cookbooks in a remote repository, such as an S3 bucket or a Git repository\. 

  For more information, see [Storing Cookbooks](#best-practices-server-migrate-cookbooks-store)\.
+ You can [execute recipes manually](workingcookbook-manual.md), but you typically have AWS OpsWorks Stacks execute recipes for you in response to a set of [lifecycle events](workingcookbook-events.md) that occur at key points during an instance's lifecycle\.

  For more information, see [Executing Recipes](#best-practices-server-migrate-cookbooks-execute)\.
+ AWS OpsWorks Stacks supports Berkshelf on Chef 11\.10 stacks only\. If you use Berkshelf to manage your cookbook dependencies, you cannot use stacks running Chef 11\.4 or earlier versions\.

  For more information, see [Using Berkshelf](workingcookbook-chef11-10.md#workingcookbook-chef11-10-berkshelf)\.

**Topics**
+ [Storing Cookbooks](#best-practices-server-migrate-cookbooks-store)
+ [Executing Recipes](#best-practices-server-migrate-cookbooks-execute)

### Storing Cookbooks<a name="best-practices-server-migrate-cookbooks-store"></a>

With Chef Server, you store your cookbooks on the server and deploy them from the server to the instances\. With AWS OpsWorks Stacks, you store cookbooks in a repository— an S3 or HTTP archive or a Git or Subversion repository\. You specify the information that AWS OpsWorks Stacks needs to download the code from the repository to a stack's instances when you [install cookbooks](workingapps-creating.md)\.

To migrate from Chef Server, you must put your cookbooks in one of these repositories\. For information on how to structure a cookbook repository, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\. 

### Executing Recipes<a name="best-practices-server-migrate-cookbooks-execute"></a>

In AWS OpsWorks Stacks, each layer has a set of [lifecycle events](workingcookbook-events.md)—Setup, Configure, Deploy, Undeploy, and Shutdown—each of which occurs at a key point during an instance's lifecycle\. To execute a custom recipe, you typically assign it to the appropriate event on the appropriate layer\. When the event occurs, AWS OpsWorks Stacks runs the associated recipes\. For example, the Setup event occurs after an instance finishes booting, so you typically assign recipes to this event that perform tasks such as installing and configuring packages and starting services\.

You can execute recipes manually by using the [Execute Recipes stack command](workingstacks-commands.md)\. This command is useful for development and testing, but you also can use it to execute recipes that don't map to a lifecycle event\. You can also use the Execute Recipes command to manually trigger Setup and Configure events\.

In addition to the AWS OpsWorks Stacks console, you can use the [AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) or [SDKs](https://aws.amazon.com/tools/) to execute recipes\. These tools support all of the [AWS OpsWorks Stacks API actions](http://docs.aws.amazon.com/opsworks/latest/APIReference/Welcome.html), but are simpler to use than the API\. Use the [create\-deployment](http://docs.aws.amazon.com/cli/latest/reference/opsworks/create-deployment.html) CLI command to trigger a lifecycle event, which runs all of the associated recipes\. You also can use this command to execute one or more recipes without triggering an event\. The equivalent SDK code depends on the particular language, but is generally similar to the CLI command\.

The following examples describe two ways to use the `create-deployment` CLI command to automate application deployment\.
+ Deploy your app on a regular schedule by adding a custom layer with a single instance to your stack\.

  Add a custom Setup recipe to the layer that creates a `cron` job on the instance to run the command on a specified schedule\. For an example of how to use a recipe to create a `cron` job, see [Running Cron Jobs on Linux Instances](workingcookbook-extend-cron.md)\.
+ Add a task to your continuous integration pipeline that uses the `create-deployment` CLI command to deploy the app\.

## Using Chef Environments<a name="best-practices-server-migrate-environments"></a>

AWS OpsWorks Stacks does not support Chef environments; `node.chef_environment` always returns `_default`\.