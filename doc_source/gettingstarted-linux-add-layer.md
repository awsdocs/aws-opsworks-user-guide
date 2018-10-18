# Step 3: Add a Layer to the Stack<a name="gettingstarted-linux-add-layer"></a>

 A *layer* is a blueprint for a set of instances, such as Amazon EC2 instances\. It specifies information such as the instance's settings, resources, installed packages, and security groups\. Next, add a layer to the stack\. \(For more information about layers, see [Layers](workinglayers.md)\.\)

**To add the layer to the stack**

1. With the **MyLinuxDemoStack** page displayed from the previous step, for **Layers**, choose **Add a layer**:   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-add-layer-console.png)

1. The **Add Layer** page is displayed\. On the **OpsWorks** tab, for **Name**, type **MyLinuxDemoLayer**\. \(You can type a different name, but be sure to substitute it for `MyLinuxDemoLayer` throughout this walkthrough\.\)

1. For **Short name**, type **demo** \(you can type a different value, but be sure to substitute it for `demo` throughout this walkthrough\):  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-add-layer-page-console.png)

1. Choose **Add layer**\. AWS OpsWorks Stacks creates the layer and displays the **Layers** page\.

1. On the **Layers** page, for **MyLinuxDemoLayer**, choose **Network**\.

1. On the **Network** tab, under **Automatically Assign IP Addresses**, verify that **Public IP addresses** is set to **yes**\. If you've made changes, choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add_layer_publicip.png)

1. On the **Layers** page, choose **Security**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-layer-page-console.png)

1. The **Layer MyLinuxDemoLayer** page is displayed with the **Security** tab open\. For **Security groups**, choose **AWS\-OpsWorks\-WebApp**, and then choose **Save**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-layer-security-console.png)

1. The `AWS-OpsWorks-WebApp` security group is added to the layer\. \(This security group enables users to connect to the app on the instance later in this walkthrough\. Without this security group, users will receive a message in their web browser that they cannot connect to the instance\.\)

You now have a layer with the correct settings for this walkthrough\.

In the [next step](gettingstarted-linux-specify-app.md), you will specify the app to deploy to the instance\. 