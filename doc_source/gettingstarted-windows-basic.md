# Step 2: Create a Basic Application Server Stack<a name="gettingstarted-windows-basic"></a>

A basic application server stack consists of a single application server instance with a public IP address to receive user requests\. Application code and any related files are stored in a separate repository and deployed from there to the server\. The following diagram illustrates such a stack\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch_2_windows.png)

The stack has the following components:
+ A *layer*, which represents a group of instances and specifies how they are to be configured\.

  The layer in this example represents a group of IIS instances\.
+ An *instance*, which represents an Amazon EC2 instance\.

  In this case, the layer configures a single instance to run IIS, but layers can have any number of instances\.
+ An *app*, which contains the information required to install an application on the instance\.
+ A *cookbook*, which contains custom Chef recipes that support the custom IIS layer\. The cookbook and app code are stored in remote repositories, such as an archive file in an Amazon S3 bucket or a Git repository\.

The following sections describe how to use the AWS OpsWorks Stacks console to create the stack and deploy the application\.

**Topics**
+ [Step 2\.1: Create the Stack](gettingstarted-windows-stack.md)
+ [Step 2\.2: Authorize RDP Access](gettingstarted-windows-rdp.md)
+ [Step 2\.3: Implement a Custom Cookbook](gettingstarted-windows-cookbook.md)
+ [Step 2\.4: Add an IIS Layer](gettingstarted-windows-iis-layer.md)
+ [Step 2\.5: Deploy an App](gettingstarted-windows-deploy.md)