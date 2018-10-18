# MySQL OpsWorks Layer<a name="workinglayers-db-mysql"></a>

**Note**  
This layer is available only for Chef 11 and earlier Linux\-based stacks\.

A MySQL OpsWorks layer provides a blueprint for Amazon EC2 instances that function as a [MySQL](http://www.mysql.com/) database master\. A built\-in recipe creates a database for each application that has been deployed to an application server layer\. For example, if you deploy a PHP application “myapp,” the recipe creates a “myapp” database\.

The MySQL layer has the following configuration settings\.

**MySQL root user password**  
\(Required\) The root user password\.

**Set root user password on every instance**  
\(Optional\) Whether the root user password is included in the stack configuration and deployment attributes that are installed on every instance in the stack\. The default setting is **Yes**\.  
If you set this value to **No**, AWS OpsWorks Stacks passes the root password only to application server instances\.

**Custom security groups**  
\(Optional\) A custom security group to be associated with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add_layer_mysql.png)

You can add one or more instances to the layer, each of which represents a separate MySQL database master\. You can then [attach an instance to an app](workingapps-creating.md), which installs the necessary connection information on the app's application servers\. The application can then use the connection information to [connect to the instance's database server](workingapps-connectdb.md)\.