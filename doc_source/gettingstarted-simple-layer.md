# Step 2\.2: Add a PHP App Server Layer \- Chef 11<a name="gettingstarted-simple-layer"></a>

Although a stack is basically a container for instances, you don't add instances directly to a stack\. You add a layer, which represents a group of related instances, and then add instances to the layer\.

A layer is basically a blueprint that AWS OpsWorks Stacks uses to create set of Amazon EC2 instances with the same configuration\. You add one layer to the stack for each group of related instances\. AWS OpsWorks Stacks includes a set of built\-in layers to represent groups of instances running standard software packages such as a MySQL database server or a PHP application server\. In addition, you can create partially or fully customized layers to suit your specific requirements\. For more information, see [Customizing AWS OpsWorks Stacks](customizing.md)\.

MyStack has one layer, the built\-in PHP App Server layer, which represents a group of instances that function as PHP application servers\. For more information, including descriptions of the built\-in layers, see [Layers](workinglayers.md)\.

**To add a PHP App Server layer to MyStack**

1. 

**Open the Add Layer Page**

   After you finish creating the stack, AWS OpsWorks Stacks displays the **Stack** page\. Click **Add a layer** to add your first layer\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs2a.png)

1. 

**Specify a Layer Type and Configure the Layer**

   In the **Layer type** box, select **PHP App Server**, accept the default **Elastic Load Balancer** setting and click **Add Layer**\. After you create the layer, you can specify other attributes such as the EBS volume configuration by [editing the layer](workinglayers-basics-edit.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs3.png)