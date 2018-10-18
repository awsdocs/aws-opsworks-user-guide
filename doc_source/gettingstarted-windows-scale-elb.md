# Add a Load Balancer<a name="gettingstarted-windows-scale-elb"></a>

Elastic Load Balancing is an AWS service that automatically distributes incoming application traffic across multiple Amazon EC2 instances\. A load balancer can serve two purposes\. The obvious one is to equalize the load on your application servers\. Many sites prefer to isolate their application servers and databases from direct user access\. In addition to distributing traffic, Elastic Load Balancing does the following:
+ Detects unhealthy Amazon EC2 instances\.

  It reroutes traffic to the remaining healthy instances until the unhealthy instances have been restored\.
+ Automatically scales request handling capacity in response to incoming traffic\.

**Note**  
AWS OpsWorks Stacks does not support Application Load Balancer\. You can only use Classic Load Balancer with AWS OpsWorks Stacks\.

Although Elastic Load Balancing is often referred to as a layer, it works a bit differently than the other built\-in layers\. Instead of creating a layer and adding instances to it, you create an Elastic Load Balancing load balancer by using the Amazon EC2 console and then attach it to one of your existing layers, usually an application server layer\. AWS OpsWorks Stacks then registers the layer's existing instances with the service and automatically adds any new instances\. The following procedure describes how to add a load balancer\.

**To attach a load balancer to the custom IIS layer**

1. Use the Amazon EC2 console to create a new load balancer for IISExample\. For more information, see [Getting Started with Elastic Load Balancing](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/load-balancer-getting-started.html)\. When you run the **Create Load Balancer** wizard, configure the load balancer as follows:  
**1: Define Load Balancer**  
Assign the load balancer an easily recognizable name, like IIS\-LB, to make it easier to locate in the AWS OpsWorks Stacks console\. Accept the defaults for the remaining settings, and then choose **Next: Assign Security Groups**\.  
**2: Assign Security Groups**  
If your account supports default VPC, the wizard displays this page to determine the load balancer's security group\. It does not display this page for EC2 Classic\.  
For this walkthrough, specify **default VPC security group**, and then choose **Next: Configure Security Settings**\.  
**3: Configure Security Settings**  
This walkthrough does require your load balancer to use a secure listener \(that is, HTTPS or SSL on its front\-end connection\), so choose **Next: Configure Health Check** to continue\.  
**4: Configure Health Check**  
Set the ping path to **/**\. Accept the defaults for the remaining settings, and then choose **Next: Add EC2 Instances**\.  
**5: Add EC2 Instances**  
AWS OpsWorks Stacks automatically takes care of registering instances with the load balancer\. Choose **Next Add Tags** to continue\.   
**6: Add Tags**  
You won't be using tags for this example\. Choose **Review and Create**\.   
**7: Review**  
Review your choices and choose **Create** and then **Close**, which launches the load balancer\.

1. If your account supports default VPC, after you launch the load balancer you must ensure that its security group has appropriate inbound rules\. The default rule does not accept any inbound traffic\.

   1. Choose **Security Groups** in the Amazon EC2 navigation pane\.

   1. Choose **default VPC security group**

   1. On the **Inbound** tab, choose **Edit** \.

   1. For this walkthrough, set **Source** to **Anywhere**, which directs the load balancer to accept incoming traffic from any IP address\. 

   1. Click **Save**\.

1. Return to the AWS OpsWorks Stacks console\. On the **Layers** page, choose **Network**\.

1. Under **Elastic Load Balancing**, select the IIS\-LB load balancer that you created in Step 1, and then click **Save**\.

   After you have attached the load balancer to the layer, AWS OpsWorks Stacks automatically registers the layer's current instances and adds new instances as they come online\.

1. On the **Layers** page, click the load balancer's name to open its details page\. A green check next to the instance on the load balancer page indicates that the instance has passed a health check\.

You can now run IIS\-Example\-App by sending a request to the load balancer\.

**To run IIS\-Example\-App through the load balancer**

1. Choose **Layers**\. The IIS\-ELB load balancer should be listed as a layer and the Health column should have one instance in green, which indicates a healthy instance\.

1. Choose the load balancer's DNS name to run IIS\-Example\-App\. It should be listed under the load balancer's name and look something like `IIS-LB-1802910859.us-west-2.elb.amazonaws.com`\. The load balancer forwards the request to the instance and returns the response, which should look exactly the same as the response you get when you click the instance's public IP address\.

You have only one instance at this point, so the load balancer isn't really adding much\. However, you can now add additional instances to the layer\.

**To add an instance to the layer**

1. Choose **Instances** and then **\+ instance** to add another instance to the layer\.

1. Start the instance\.

Because they are new instances, AWS OpsWorks Stacks automatically installs the current custom cookbooks and deploys the current app version during setup\. When the instance comes online, AWS OpsWorks Stacks automatically adds it to the load balancer, so your instance will immediately start handling requests\. To verify that the application is still working, you can choose the load balancer's DNS name again \.