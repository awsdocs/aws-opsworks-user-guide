# Using a Back\-end Data Store<a name="customizing-rds"></a>

Application server stacks commonly include a database server to provide a back\-end data store\. AWS OpsWorks Stacks provides integrated support for MySQL servers through the [MySQL layer](workinglayers-db-mysql.md) and for several types of database servers through the [Amazon Relational Database Service \(Amazon RDS\) layer](workinglayers-db-rds.md)\. However, you can easily customize a stack to have the application servers use other database servers such as Amazon DynamoDB or MongoDB\. This topic describes the basic procedure for connecting an application server to an AWS database server\. It uses the stack and application from [Getting Started with Chef 11 Linux Stacks](gettingstarted.md) to show how to manually connect a PHP application server to an RDS database\. Although the example is based on a Linux stack, the basic principles also apply to Windows stacks\. For an example of how to incorporate a MongoDB database server into a stack, see [Deploying MongoDB with OpsWorks](https://aws.amazon.com/blogs/devops/deploying-mongodb-with-opsworks/)\.

**Note**  
This topic uses Amazon RDS as a convenient example\. However, if you want to use an Amazon RDS database with your stack, it's much easier to use an Amazon RDS layer\. 

**Topics**
+ [How to Set up a Database Connection](customizing-rds-setup.md)
+ [How to Connect an Application Server Instance to Amazon RDS](customizing-rds-connect.md)