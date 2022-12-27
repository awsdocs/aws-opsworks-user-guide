# Limitations<a name="migrating-to-systems-manager-limitations"></a>

The new OpsWorks architecture differs from the architecture for AWS OpsWorks Stacks\. This section describes the known limitations of this architecture\.

The following are not supported by the new OpsWorks architecture\.
+ Running Chef recipes on Windows and CentOS instances
+ Built\-in Chef 11 layers
+ Chef attributes and data bags
+ On\-premises instances
+ Instances imported from EC2
+ No support for installing a user specified list of operating system packages
+ Apps are not supported or migrated

The following are supported with limitations\.
+ The migration script clones EBS volume information, but excludes mount points and actual data contained in the volumes\.
+ Time\-based and load\-based scaled instances are migrated, but any scaling rules associated with these instances are not migrated\. You can modify the Auto Scaling group to achieve similar results\.
+ IAM entities defined in the stack's **Permissions** page in the OpsWorks console are not created or generated\.
+  The migration script is only capable of provisioning single layer applications in Systems Manager\. For example, if you run the script twice for two layers in the same stack, you get two different applications in Systems Manager\. 