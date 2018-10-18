# Amazon RDS Service Layer<a name="workinglayers-db-rds"></a>

An Amazon RDS service layer represents an Amazon RDS instance\. The layer can represent only existing Amazon RDS instances, which you must create separately by using the [Amazon RDS console](https://console.aws.amazon.com/rds/) or API\. 

The basic procedure for incorporating an Amazon RDS service layer into your stack is as follows:

1. Use the Amazon RDS console, API, or CLI to create an instance\.

   Be sure to record the instance's ID, master user name, master password, and database name\.

1. To add an Amazon RDS layer to your stack, register the Amazon RDS instance with the stack\.

1. Attach the layer to an app, which adds the Amazon RDS instance's connection information to the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy)\.

1. Use the language\-specific files or the information in the `deploy` attributes to connect the application to the Amazon RDS instance\.

   For more information on how to connect an application to a database server, see [Connecting an Application to a Database Server](workingapps-connectdb.md)

**Warning**  
Be sure that the characters in the instance's master password and user name are compatible with your application server\. For example, with the Java App Server layer, including `&` in either string causes an XML parsing error that prevents the Tomcat server from starting up\. 

**Topics**
+ [Specifying Security Groups](#workinglayers-db-rds-security)
+ [Registering an Amazon RDS Instance with a Stack](#workinglayers-db-rds-register)
+ [Associating Amazon RDS Service Layers with Apps](#workinglayers-db-rds-register-attach)
+ [Removing an Amazon RDS Service Layer from a Stack](#workinglayers-db-rds-register-remove)

## Specifying Security Groups<a name="workinglayers-db-rds-security"></a>

To use an Amazon RDS instance with AWS OpsWorks Stacks, the database or VPC security groups must allow access from the appropriate IP addresses\. For production use, a security group usually limits access to only those IP addresses that need to access the database\. It typically includes the addresses of the systems that you use to manage the database and the AWS OpsWorks Stacks instances that need to access the database\. AWS OpsWorks Stacks automatically creates an Amazon EC2 security group for each type of layer when you create your first stack in a region\. A simple way to provide access for AWS OpsWorks Stacks instances is to assign the appropriate AWS OpsWorks Stacks security groups to the Amazon RDS instance or VPC\.

**To specify security groups for an existing Amazon RDS instance**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Click **Instances** in the navigation pane and select the appropriate Amazon RDS instance\. Click **Instance Actions**, **Modify**\.

1. Select the following security groups from the **Security Group** list and then click **Continue** and **Modify DB Instance** to update the instance\.
   + The **AWS\-OpsWorks\-DB\-Master\-Server \(*security\_group\_id*\)** security group\.
   + The security group for the app server layer whose instances will be connecting to the database\. The group name includes the layer name\. For example, to provide database access to PHP App Server instances, specify the **AWS\-OpsWorks\-PHP\-App\-Server** group\.

If you are creating a new Amazon RDS instance, you can specify the appropriate AWS OpsWorks Stacks security groups on the Launch DB Instance wizard's **Configure Advanced Settings** page\. For a description of how to use this wizard, see [Creating a MySQL DB Instance and Connecting to a Database on a MySQL DB Instance](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html)\.

For information on how to specify VPC security groups, see [Security Groups for Your VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html)\.

## Registering an Amazon RDS Instance with a Stack<a name="workinglayers-db-rds-register"></a>

To add an Amazon RDS service layer in a stack, you must register an instance with the stack\. 

**To register an Amazon RDS instance with a stack**

1. In the AWS OpsWorks Stacks console, click **Layer** in the navigation pane, click **\+ Layer** or **Add a layer** to open the **Add Layer** page, and then click the **RDS** tab\.

1. If necessary, update the stack's service role, as described in [Updating the Stack's Service Role](#workinglayers-db-rds-register-role)\.

1. Click the RDS tab to list the available Amazon RDS instances\.
**Note**  
If your account does not have any Amazon RDS instances, you can create one by clicking **Add an RDS instance** on the RDS tab, which takes you to the Amazon RDS console and starts the **Launch a DB Instance** wizard\. You can also go directly to the [Amazon RDS console](https://console.aws.amazon.com/rds/) and click **Launch a DB Instance**, or use the Amazon RDS API or CLI\. For more information on how to create an Amazon RDS instance, see [Getting Started with Amazon RDS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html)\.

1. Select the appropriate instance, set **User** and **Password** to the appropriate user and password values and click **Register to Stack**\.
**Important**  
You must ensure that the user and password that you use to register the Amazon RDS instance correspond to a valid user and password\. If they do not, your applications will not be able connect to the instance\. However, you can [edit the layer](workinglayers-basics-edit.md) to provide valid user and password values and then redeploy the app\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/rds-register.png)

When you add an Amazon RDS service layer to a stack, AWS OpsWorks Stacks assigns it an ID and adds the associated Amazon RDS configuration to the [stack configuration and deployment attribute's](workingcookbook-json.md) [`[:opsworks][:stack]`](attributes-json-opsworks-stack.md) attribute\.

**Note**  
If you change a registered Amazon RDS instance's password, you must manually update the password in AWS OpsWorks Stacks and then redeploy your apps to update the stack configuration and deployment attributes on the stack's instances\. 

**Topics**
+ [Updating the Stack's Service Role](#workinglayers-db-rds-register-role)

### Updating the Stack's Service Role<a name="workinglayers-db-rds-register-role"></a>

Every stack has an [IAM service role](opsworks-security-servicerole.md) that specifies what actions AWS OpsWorks Stacks can perform on your behalf with other AWS services\. To register an Amazon RDS instance with a stack, its service role must grant AWS OpsWorks Stacks permissions to access Amazon RDS\. 

The first time you add an Amazon RDS service layer to one of your stacks, the service role might lack the required permissions\. If so, when you click the RDS tab on the **Add Layer** page, you will see the following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/rds-iam-update.png)

Click **Update** to have AWS OpsWorks Stacks update the service role's policy to the following\.

```
{"Statement": [{"Action": ["ec2:*", "iam:PassRole",
                       "cloudwatch:GetMetricStatistics",
                       "elasticloadbalancing:*",
                       "rds:*"],
            "Effect": "Allow",
            "Resource": ["*"] }]
}
```

**Note**  
You need to perform the update only once\. The updated role is then automatically used by all of your stacks\.

## Associating Amazon RDS Service Layers with Apps<a name="workinglayers-db-rds-register-attach"></a>

After you add an Amazon RDS service layer, you can associate it with an app\. 
+ You can associate an Amazon RDS layer to an app when you [create the app](workingapps-creating.md), or later by [editing the app's configuration](workingapps-editing.md)\.
+ To disassociate an Amazon RDS layer from an app, edit the app's configuration to specify a different database server, or no server\.

  The Amazon RDS layer remains part of the stack, and can be associated with a different app\.

After you associate an Amazon RDS instance with an app, AWS OpsWorks Stacks puts the database connection information on the app's servers\. The application on each server instance can then use this information to connect to the database\. For more information on how to connect to an Amazon RDS instance, see [Connecting an Application to a Database Server](workingapps-connectdb.md)\. 

## Removing an Amazon RDS Service Layer from a Stack<a name="workinglayers-db-rds-register-remove"></a>

To remove an Amazon RDS service layer from a stack, you deregister it\.

**To deregister an Amazon RDS service layer**

1. Click **Layers** in the navigation pane and click the Amazon RDS service layer's name\.

1. Click **Deregister** and confirm that you want to deregister the layer\.

This procedure removes the layer from the stack, but it does not delete the underlying Amazon RDS instance\. The instance and any databases remain in your account and can be registered with other stacks\. You must use the Amazon RDS console, API, or CLI to delete the instance\. For more information, see [Deleting a DB Instance](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html#CHAP_GettingStarted.Deleting)\.