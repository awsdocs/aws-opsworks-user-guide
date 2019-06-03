# Step 2\.4: Add an IIS Layer<a name="gettingstarted-windows-iis-layer"></a>

Your cookbook has one recipe that just installs and starts IIS\. This is enough to create the layer and verify that you have a working IIS instance\. Later, you will add application deployment functionality to the layer\.

## Create a Layer<a name="w4ab1c11c39c15c21c21b5"></a>

You start by adding a layer to the stack\. You then add functionality to that layer by assigning custom recipes to the appropriate lifecycle events\.

**To add an IIS layer to the stack**

1. Choose **Layers** in the navigation pane and then choose **Add a layer**\.

1. Configure the layer as follows:
   + **Name**– **IISExample** 
   + **Short name** – **iisexample**

     AWS OpsWorks Stacks uses the short name to identify the layer internally\. You also use the short name to identify the layer in recipes, although this example does not do so\. You can specify any short name, but it can consist only of lowercase alphanumeric characters and a small number of punctuation marks\. For more information, see [Custom Layers](workinglayers-custom.md)\.

1. Choose **Add Layer**\.

If you were to add an instance to IISWalkthrough at this point and start it, AWS OpsWorks Stacks would automatically install the cookbooks but it would not run `install.rb`\. After an instance is online, you can run recipes manually by using the [Execute Recipes stack command](workingstacks-commands.md)\. However, a better approach is to assign the recipe to one of the layer's [lifecycle events](workingcookbook-events.md)\. AWS OpsWorks Stacks then automatically runs the recipe at the appropriate point in the instance's lifecycle\.

Install and start IIS as soon as the instance finishes booting\. To do this, assign `install.rb` to the layer's `Setup` event\.

**To assign the recipe to a lifecycle event**

1. Choose **Layers** in the navigation pane

1. In the box for the **IISExample** layer, choose **Recipes**\.

1. On the upper right, choose **Edit**\.

1. Under **Custom Chef Recipes**, in the **Setup** recipes box, type **iis\-cookbook::install**\. 
**Note**  
Use `cookbook-name::recipe-name` to identify recipes, where you omit the recipe name's `.rb` suffix\.

1. Choose **\+** to add the recipe to the layer\. A red x appears next to the recipe to make it easy to remove later\.

1. Choose **Save** to save the new configuration\. The custom Setup recipes should now include `iis-cookbook::install`\.

## Add an Instance to the Layer and Start It<a name="w4ab1c11c39c15c21c21b7"></a>

You can try the recipe out by adding an instance to the layer and starting the instance\. AWS OpsWorks Stacks automatically installs the cookbooks and runs `install.rb` during setup, as soon as the instance finishes booting\.

**To add an instance to a layer and start it**

1. In the AWS OpsWorks Stacks navigation pane, choose **Instances**\.

1. Under **IISExample** layer, choose **Add an instance**\. 

1. Select the appropriate size\. **t2\.micro** \(or the smallest size available to you\) should be sufficient for the example\.

1. Choose **Add Instance**\. By default, AWS OpsWorks Stacks generates instance names by appending an integer to the layer's short name, so the instance should be named **iisexample1**\.

1. Choose **start** in the instance's **Actions** column to start the instance\. AWS OpsWorks Stacks will then launch an EC2 instance and run the Setup recipes to configure it\. If the layer had any Deploy recipes at this point, AWS OpsWorks Stacks would run them after the Setup recipes have finished\.

   The process may take a number of minutes, during which the **Status** column displays a series of status states\. When you get to **online** status, the setup process is complete and the instance is ready for use\.

## Confirm that IIS is Installed and Running<a name="w4ab1c11c39c15c21c21b9"></a>

You can use RDP to connect to the instance and verify that your Setup recipe worked correctly\.

**To verify that IIS is installed and running**

1. Choose **Instances** in the navigation pane and choose **rdp** in the **iisexample1** instance's **Actions** column\. AWS OpsWorks Stacks automatically generates an RDP password for you that expires after a specified time period\.

1. Set **Session valid for** to 2 hours and choose **Generate Password**\.

1. AWS OpsWorks Stacks displays the password and also, for your convenience, the instance's public DNS name and user name\. Copy all three and click **Acknowledge and close**\.

1. Open your RDP client and use the data from Step 3 to connect to the instance\.

1. On the instance, open Windows Explorer and examine the `C:` drive\. It should have a `C:\inetpub` directory, which was created by the IIS installation\.

1. Open the Control Panel **Administrative Tools** application, and then open **Services**\. You should see the IIS service near the bottom of the list\. It is named World Wide Web Publishing Service, and the status should be **running**\.

1. Return to the AWS OpsWorks Stacks console and choose the **iisexample1** instance's public IP address\. Be sure you do this in AWS OpsWorks Stacks, and not in the Amazon EC2 console\. This automatically sends an HTTP request to the address, which should open the default IIS Welcome page\.

The next topic discusses how to deploy an app to the instance, a simple static HTML page for this example\. However, if you would like to take a break, choose **stop** in the **iisexample1** instance's **Actions** column to stop the instance and avoid incurring unnecessary charges\. You can restart the instance when you are ready to continue\.