# Using Stack Configuration and Deployment Attribute Values<a name="cookbooks-101-opsworks-opsworks-stack-config"></a>

Recipes often need information about the stack configuration or deployed apps\. For example, you might need a list of the stack's IP addresses to create a configuration file, or an app's deployment directory to create a log directory\. Instead of storing this data on a central server, AWS OpsWorks Stacks installs a set of stack configuration and deployment attributes in each instance's node object for each lifecycle event\. These attributes represent the current stack state, including deployed apps\. Recipes can then obtain the data they need from the node object\.

**Note**  
Applications sometimes need information from the node object, such as stack configuration and deployment attribute values\. However, an application cannot access the node object\. To provide node object data to an application, you can implement a recipe that retrieves the required information from the node object and puts it in a file in a convenient format\. The application can then read the data from the file\. For more information and an example, see [Passing Data to Applications](apps-data.md)\.

Recipes can obtain stack configuration and deployment attribute values from the node object as follows\.
+ Directly, by using an attribute's fully qualified name\.

  You can use this approach with any Linux stack, but not with Windows stacks\.
+ With Chef search, which you can use to query the node object for attribute values\.

  You can use this approach with Windows stacks and Chef 11\.10 Linux stacks\.

**Note**  
With Linux stacks, you can use the agent CLI to get a copy of an instance's stack configuration and deployment attributes in JSON format\. For more information, see [Mocking the Stack Configuration and Deployment Attributes on Vagrant](opsworks-opsworks-mock.md)\.

**Topics**
+ [Obtaining Attribute Values Directly](cookbooks-101-opsworks-opsworks-stack-config-node.md)
+ [Obtaining Attribute Values with Chef Search](cookbooks-101-opsworks-opsworks-stack-config-search.md)