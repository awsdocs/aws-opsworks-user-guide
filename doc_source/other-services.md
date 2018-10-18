# Using AWS OpsWorks Stacks with Other AWS Services<a name="other-services"></a>

You can have application servers running in an AWS OpsWorks Stacks stack use a variety of AWS services that are not directly integrated with AWS OpsWorks Stacks\. For example, you can have your application servers use Amazon RDS as a back\-end database\. You can access such services by using the following general pattern:

1. Create and configure the AWS service by using the AWS console, API, or CLI and record any required configuration data that the application will need to access the service, such as host name or port\.

1. Create one or more custom recipes to configure the application so that it can access the service\.

   The recipe obtains the configuration data from [stack configuration and deployment JSON](workingcookbook-json.md) attributes that you define with custom JSON prior to running the recipes\.

1. Assign the custom recipe to the Deploy lifecycle event on the application server layer\.

1. Create a custom JSON object that assigns appropriate values to the configuration data attributes and add it to your stack configuration and deployment JSON\.

1. Deploy the application to the stack\. 

   Deployment runs the custom recipes, which use the configuration data values that you defined in the custom JSON to configure the application so that it can access the service\.

This section describes how to have AWS OpsWorks Stacks application servers access a variety of AWS services\. It assumes that you are already familiar with Chef cookbooks and how recipes can use stack and configuration JSON attributes to configure applications, typically by creating configuration files\. If not, you should first read [Cookbooks and Recipes](workingcookbook.md) and [Customizing AWS OpsWorks Stacks](customizing.md)\.

**Topics**
+ [Using a Back\-end Data Store](customizing-rds.md)
+ [Using ElastiCache Redis as an In\-Memory Key\-Value Store](other-services-redis.md)
+ [Using an Amazon S3 Bucket](gettingstarted.walkthrough.photoapp.md)
+ [Using AWS CodePipeline with AWS OpsWorks Stacks](other-services-cp.md)