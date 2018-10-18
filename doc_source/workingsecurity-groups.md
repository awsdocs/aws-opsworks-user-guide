# Using Security Groups<a name="workingsecurity-groups"></a>

## Security Groups<a name="bestpractice-secgroups"></a>

Each Amazon EC2 instance has one or more associated security groups that govern the instance's network traffic, much like a firewall\. A security group has one or more *rules*, each of which specifies a particular category of allowed traffic\. A rule specifies the following:
+ The type of allowed traffic, such as SSH or HTTP
+ The traffic's protocol, such as TCP or UDP
+ The IP address range that the traffic can originate from
+ The traffic's allowed port range

Security groups have two types of rules:
+ Inbound rules govern inbound network traffic\.

  For example, application server instances commonly have an inbound rule that allows inbound HTTP traffic from any IP address to port 80, and another inbound rule that allows inbound SSH traffic to port 22 from specified set of IP addresses\.
+ Outbound rules govern outbound network traffic\.

  A common practice is to use the default setting, which allows any outbound traffic\.

For more information about security groups, see [Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)\.

The first time you create a stack in a region, AWS OpsWorks Stacks creates a built\-in security group for each layer with an appropriate set of rules\. All of the groups have default outbound rules, which allow all outbound traffic\. In general, the inbound rules allow the following:
+ Inbound TCP, UDP, and ICMP traffic from the appropriate AWS OpsWorks Stacks layers
+ Inbound TCP traffic on port 22 \(SSH login\)
**Warning**  
 The default security group configuration opens SSH \(port 22\) to any network location \(0\.0\.0\.0/0\.\) This allows all IP addresses to access your instance by using SSH\. For production environments, you must use a configuration that only allows SSH access from a specific IP address or range of addresses\. Either update the default security groups immediately after they are created, or use custom security groups instead\. 
+ For web server layers, all inbound TCP, and UDP traffic to ports 80 \(HTTP\) and 443 \(HTTPS\)

**Note**  
The built\-in `AWS-OpsWorks-RDP-Server` security group is assigned to all Windows instances to allow RDP access\. However, by default, it does not have any rules\. If you are running a Windows stack and want to use RDP to access instances, you must add an inbound rule that allows RDP access\. For more information, see [Logging In with RDP](workinginstances-rdp.md)\.

To see the details for each group, go to the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), select **Security Groups** in the navigation pane, and select the appropriate layer's security group\. For example, **AWS\-OpsWorks\-Default\-Server** is the default built\-in security group for all stacks, and **AWS\-OpsWorks\-WebApp** is the default built\-in security group for the Chef 12 sample stack\.

**Note**  
If you accidentally delete an AWS OpsWorks Stacks security group, the preferred way to recreate it is to have AWS OpsWorks Stacks perform the task for you\. Just create a new stack in the same AWS region—and VPC, if present—and AWS OpsWorks Stacks will automatically recreate all the built\-in security groups, including the one that you deleted\. You can then delete the stack if you don't have any further use for it; the security groups will remain\. If you want to recreate the security group manually, it must be an exact duplicate of the original, including the group name's capitalization\.  
Additionally, AWS OpsWorks Stacks will attempt to recreate all built\-in security groups if any of the following occur:  
You make any changes to the stack's settings page in the AWS OpsWorks Stacks console\.
You start one of the stack's instances\.
You create a new stack\.

You can use either of the following approaches for specifying security groups\. You use the **Use OpsWorks security groups** setting to specify your preference when you create a stack\. 
+ **Yes** \(default setting\) – AWS OpsWorks Stacks automatically associates the appropriate built\-in security group with each layer\.

  You can fine\-tune a layer's built\-in security group by adding a custom security group with your preferred settings\. However, when Amazon EC2 evaluates multiple security groups, it uses the least restrictive rules, so you cannot use this approach to specify more restrictive rules than the built\-in group\.
+ **No** – AWS OpsWorks Stacks does not associate built\-in security groups with layers\.

  You must create appropriate security groups and associate at least one with each layer that you create\. Use this approach to specify more restrictive rules than the built\-in groups\. Note that you can still manually associate a built\-in security group with a layer if you prefer; custom security groups are required only for those layers that need custom settings\.

**Important**  
If you use the built\-in security groups, you cannot create more restrictive rules by manually modifying the group's settings\. Each time you create a stack, AWS OpsWorks Stacks overwrites the built\-in security groups' configurations, so any changes that you make will be lost the next time you create a stack\. If a layer requires more restrictive security group settings than the built\-in security group, set **Use OpsWorks security groups** to **No**, create custom security groups with your preferred settings, and assign them to the layers on creation\.