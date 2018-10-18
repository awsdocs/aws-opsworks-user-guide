# Step 4\.1: Add a Load Balancer<a name="gettingstarted-scale-elb"></a>

Elastic Load Balancing is an AWS service that automatically distributes incoming application traffic across multiple Amazon EC2 instances\. In addition to distributing traffic, Elastic Load Balancing does the following:
+ Detects unhealthy Amazon EC2 instances\.

  It reroutes traffic to the remaining healthy instances until the unhealthy instances have been restored\.
+ Automatically scales request handling capacity in response to incoming traffic

**Note**  
A load balancer can serve two purposes\. The obvious one is to equalize the load on your application servers\. In addition, many sites prefer to isolate their application servers and databases from direct user access\. With AWS OpsWorks Stacks, you can do this by running your stack in a virtual private cloud \(VPC\) with a public and private subnet, as follows\.   
Put the application servers and database in the private subnet, where they can be accessed by other instances in the VPC but not by users\.
Direct user traffic to a load balancer in the public subnet, which then forwards the traffic to the application servers in the private subnet and returns responses to users\.
For more information, see [Running a Stack in a VPC](workingstacks-vpc.md)\. For an AWS CloudFormation template that extends the example in this walkthrough to run in a VPC, see [OpsWorksVPCELB\.template](https://s3.amazonaws.com/cloudformation-templates-us-east-1/OpsWorksVPCELB.template)\.

Although Elastic Load Balancing is often referred to as a layer, it works a bit differently than the other built\-in layers\. Instead of creating a layer and adding instances to it, you create an Elastic Load Balancing load balancer by using the Amazon EC2 console and then attach it to one of your existing layers, usually an application server layer\. AWS OpsWorks Stacks then registers the layer's existing instances with the service and automatically adds any new instances\. The following procedure describes how to add a load balancer to MyStack's PHP App Server layer\.

**Note**  
AWS OpsWorks Stacks does not support Application Load Balancer\. You can only use Classic Load Balancer with AWS OpsWorks Stacks\.

**To attach a load balancer to the PHP App Server layer**

1. Use the Amazon EC2 console to create a new load balancer for MyStack\. The details depend on whether your account supports EC2 Classic\. For more information, see [Getting Started with Elastic Load Balancing](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/load-balancer-getting-started.html)\. When you run the **Create Load Balancer** wizard, configure the load balancer as follows:  
**Define Load Balancer**  
Assign the load balancer an easily recognizable name, like PHP\-LB, to make it easier to locate in the AWS OpsWorks Stacks console\. Then choose **Continue** to accept defaults for the remaining settings\.  
If you choose a VPC with one or more subnets from the **Create LB Inside** menu, you must select a subnet for each availability zone where you want traffic to be routed by your load balancer\.  
**Assign Security Groups**  
If your account supports default VPC, the wizard displays this page to determine the load balancer's security group\. It does not display this page for EC2 Classic\.  
For this walkthrough, choose **default VPC security group**\.  
**Configure Security Settings**  
If you chose **HTTPS** as the **Load Balancer Protocol** on the **Define Load Balancer** page, configure certificate, cipher, and SSL protocol settings on this page\. For this walkthrough, accept defaults, and choose **Configure Health Check**\.  
**Configure Health Check**  
Set the ping path to **/** and accept defaults for remaining settings\.  
**Add EC2 Instances**  
Choose **Continue**; AWS OpsWorks Stacks automatically registers instances with the load balancer\.  
**Add Tags**  
Add tags to help you find \. Each tag is a key and value pair; for example, you could specify **Description** as the key and **Test LB** as the value for the purposes of the walkthrough\.  
**Review**  
Review your choices, choose **Create**, and then choose **Close**, which starts the load balancer\.

1. If your account supports default VPC, after you start the load balancer, you must ensure that its security group has appropriate ingress rules\. The default rule does not accept any inbound traffic\.

   1. Choose **Security Groups** in the Amazon EC2 navigation pane\.

   1. Select **default VPC security group**

   1. Choose **Edit** on the **Inbound** tab\.

   1. For this walkthrough, set **Source** to **Anywhere**, which directs the load balancer to accept incoming traffic from any IP address\.

1. Return to the AWS OpsWorks Stacks console\. On the **Layers** page, choose the layer's **Network** link, and then choose **Edit**\.

1. Under **Elastic Load Balancing**, choose the load balancer that you created in Step 1, and then choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elb_select.png)

   After you have attached the load balancer to the layer, AWS OpsWorks Stacks automatically registers the layer's current instances, and adds new instances as they come online\.

1. On the **Layers** page, click the load balancer's name to open its details page\. When registration is complete and the instance passes a health check, AWS OpsWorks Stacks shows a green check mark next to the instance on the load balancer page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elb_properties3.png)

You can now run SimplePHPApp by sending a request to the load balancer\.

**To run SimplePHPApp through the load balancer**

1. Open load balancer's details page again, if it is not already open\.

1. On the properties page, verify the instance's health\-check status and click the load balancer's DNS name to run SimplePHPApp\. The load balancer forwards the request to the PHP App Server instance and returns the response, which should look exactly the same as the response you get when you click the PHP App Server instance's public IP address\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/elb_properties2.png)

**Note**  
AWS OpsWorks Stacks also supports the HAProxy load balancer, which might have advantages for some applications\. For more information, see [HAProxy AWS OpsWorks Stacks Layer](layers-haproxy.md)\.