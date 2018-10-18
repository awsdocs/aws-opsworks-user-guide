# Update a Stack<a name="workingstacks-edit"></a>

After you have created a stack, you can update the configuration at any time\. On the **Stack** page, click **Stack Settings**, and then click **Edit**, which displays the **Settings** page\. Make the changes that you want and click **Save**\.

The settings are the same as those discussed in [Create a New Stack](workingstacks-creating.md)\. Refer to that topic for details\. However, note the following:
+ You cannot modify the region or VPC ID\.
+ If your stack is running in a VPC, the settings include a **Default subnet** setting, which lists the VPC's subnets\. If your stack is not running in a VPC, the setting is labeled **Default Availability Zones**, and lists the region's Availability Zones\.
+ You can change the default operating system, but you cannot specify a Linux operating system for a Windows stack, or Windows for a Linux stack\.
+ If you change any of the default instance settings, such as **Hostname theme** or **Default SSH key**, the new values apply only to any new instances you create, not to existing instances\.
+ Changing the **Name** changes the name that is displayed by the console; it does not change the underlying short name that AWS OpsWorks Stacks uses to identify the stack\.
+ Before you change **Use OpsWorks security groups** from **Yes** to **No**, each layer must have at least one security group in addition to the layer's built\-in security group\. For more information, see [Editing an OpsWorks Layer's Configuration](workinglayers-basics-edit.md)\. 

  AWS OpsWorks Stacks then deletes the built\-in security groups from every layer\.
+ If you change **Use OpsWorks security groups** from **No** to **Yes**, AWS OpsWorks Stacks adds the appropriate built\-in security group to each layer but does not delete the existing security groups\.