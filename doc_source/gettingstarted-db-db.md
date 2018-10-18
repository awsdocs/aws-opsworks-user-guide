# Step 3\.1: Add a Back\-end Database<a name="gettingstarted-db-db"></a>

The new version of SimplePHPApp stores its data in a back\-end database\. AWS OpsWorks Stacks supports two types of database servers:
+ The [MySQL AWS OpsWorks Stacks layer](workinglayers-db-mysql.md) is a blueprint for creating Amazon EC2 instances that host a MySQL database master\.
+ The Amazon RDS service layer provides a way to incorporate an [Amazon RDS instance](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) into a stack\.

You can also use other databases, such as Amazon DynamoDB, or create a custom layer to support databases such as [MongoDB](http://www.mongodb.org/)\. For more information, see [Using a Back\-end Data Store](customizing-rds.md)\.

This example uses a MySQL layer\.

**To add a MySQL layer to MyStack**

1. On the **Layers** page, click **\+ Layer**\.

1. On the **Add Layer** page, for **Layer type**, select **MySQL**, accept the default settings, and click **Add Layer**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gsb3.png)

**To add an instance to the MySQL layer**

1. On the **Layers** page's **MySQL** row, click **Add an instance**\.

1. On the **Instances** page, under **MySQL**, click **Add an instance**\.

1. Accept the defaults and click **Add instance**, but don't start it yet\.

**Note**  
AWS OpsWorks Stacks automatically creates a database named using the app's short name, simplephpapp for this example\. You'll need this name if you want to use [Chef recipes](http://docs.chef.io/recipes.html) to interact with the database\.