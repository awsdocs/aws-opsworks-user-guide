# Managing Registered Instances<a name="registered-instances-manage"></a>

**Note**  
This feature is supported only for Linux stacks\.

When you register an instance, it becomes an AWS OpsWorks Stacks instance, and you can manage it in much the same way as instances created with AWS OpsWorks Stacks\. There are two primary differences:
+ Registered instances do not have to be assigned to a layer\.
+ You can deregister a registered instance and return it to your direct control\.

After you register an instance, it is in the Registered state\. AWS OpsWorks Stacks provides the following management functionality to all registered instances:
+ **Health checks** – AWS OpsWorks Stacks monitors the agent to evaluate whether the instance continues to function\.

  If an instance fails a health check, AWS OpsWorks Stacks [autoheals](workinginstances-autohealing.md) registered Amazon EC2 instances and changes the status of registered on\-premises instances to `connection lost`\.
+ **[CloudWatch monitoring](monitoring-cloudwatch.md)** – CloudWatch monitoring is enabled for registered instance\.

  You can monitor metrics such as CPU utilization and available memory and optionally receive a notification if a metric crosses a specified threshold\.
+ **User management** – AWS OpsWorks Stacks provides a simple way to specify which users can access the instance and what operations they are allowed to perform\. For more information, see [Managing User Permissions](opsworks-security-users.md)\.
+ **Recipe execution** – You can use the [Execute Recipes stack command](workingstacks-commands.md) to execute Chef recipes on the instance\.
+ **Operating system updates** – You can use the [Update Dependencies stack command](workingstacks-commands.md) to update the instance's operating system\.

To take full advantage of AWS OpsWorks Stacks management functionality, you can assign the instance to a layer\. For more information, see [Assigning a Registered Instance to a Layer](registered-instances-assign.md)\.

There are differences between the way AWS OpsWorks Stacks manages Amazon EC2 and on\-premises instances\.

Amazon EC2 Instances  
+ If you stop a registered Amazon EC2 instance, AWS OpsWorks Stacks terminates instance store\-backed instances and stops Amazon EBS\-backed instances\.

  The instance is still registered with the stack and assigned to its layers, so you can restart it if needed\. You must deregister a registered instance to remove it from a stack, either [explicitly](registered-instances-deregister.md) or by [deleting the instance](workinginstances-delete.md), which automatically deregisters it\.
+ If you restart a registered Amazon EC2 instance or the instance fails and is autohealed, the result is the same as stopping and restarting the instance by using Amazon EC2\. Note these differences:
  + Instance store\-backed instances – AWS OpsWorks Stacks starts a new instance with the same AMI\.

    Note that AWS OpsWorks Stacks has no knowledge of any operations that you performed on the instance before it was registered, such as installing software packages\. If you want AWS OpsWorks Stacks to install packages or perform other configuration tasks at startup, you must provide custom Chef recipes that perform the required tasks and assign them to the appropriate layers' Setup events\.
  + Amazon EBS\-backed instances – AWS OpsWorks Stacks starts a new instance with the same AMI and reattaches the root volume, which restores the instance to its previous configuration\.
+ If you deregister a registered Amazon EC2 instance, it returns to being a regular Amazon EC2 instance\.

On\-premises Instances  
+ AWS OpsWorks Stacks cannot stop or start a registered on\-premises instance\.

  Unassigning a registered on\-premises instance triggers a Shutdown event\. However, that event simply runs the assigned layers' Shutdown recipes\. They perform tasks such as shutting down services, but do not stop the instance\.
+ AWS OpsWorks Stacks cannot autoheal a registered on\-premises instance if it fails, but the instance is marked as connection lost\.
+ On\-premises instances cannot use the Elastic Load Balancing, Amazon EBS, or Elastic IP address services\.