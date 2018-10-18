# Editing an OpsWorks Layer's Configuration<a name="workinglayers-basics-edit"></a>

After you create a layer, some properties \(such as AWS region\) are immutable, but you can change most of the layer configuration at any time\. Editing the layer also provides access to configuration settings that are not available on the **Add Layer** page\. The settings take effect as soon as you save the new configuration\.

**To edit an OpsWorks layer**

1.  In the navigation pane, click **Layers**\. 

1. On the **Layers** page, choose a layer name to open the details page, which shows the current configuration\.
**Note**  
Choosing one of the names under the layer name takes you directly to the associated tab on the details page\.

1.  Click **Edit** and then select the appropriate tab: **General Settings**, **Recipes**, **Network**, **EBS Volumes**, or **Security**\. 

The following sections describe the settings on the various tabs that are available to all layers\. Some layers have additional layer\-specific settings, which appear at the top of the page\. In addition, some settings are available only for Linux\-based stacks, as noted\. 

**Topics**
+ [General Settings](#workinglayers-basics-edit-general)
+ [Recipes](#workinglayers-basics-edit-recipes)
+ [Network](#workinglayers-basics-edit-network)
+ [EBS Volumes](#workinglayers-basics-edit-ebs)
+ [Security](#workinglayers-basics-edit-security)
+ [CloudWatch Logs](#w4ab1c11c45c19b9c21)
+ [Tags](#w4ab1c11c45c19b9c23)

## General Settings<a name="workinglayers-basics-edit-general"></a>

All layers have the following settings:

**Auto healing enabled**  
Whether [auto healing](workinginstances-autohealing.md) is enabled for the layer's instances\. The default setting is **Yes**\.

**Custom JSON**  
Data in JSON format that is passed to your Chef recipes for all instances in this layer\. You can use this, for example, to pass data to your own recipes\. For more information, see [Using Custom JSON](workingstacks-json.md)\.  
You can declare custom JSON at the deployment, layer, and stack levels\. You may want to do this if you want some custom JSON to be visible across the stack or only to an individual deployment\. Or, for example, you may want to temporarily override custom JSON declared at the layer level with custom JSON declared at the deployment level\. If you declare custom JSON at multiple levels, custom JSON declared at the deployment level overrides any custom JSON declared at both the layer and stack levels\. Custom JSON declared at the layer level overrides any custom JSON declared only at the stack level\.  
To use the AWS OpsWorks Stacks console to specify custom JSON for a deployment, on the **Deploy App** page, choose **Advanced**\. Type the custom JSON in the **Custom Chef JSON** box, and then choose **Save**\.  
To use the AWS OpsWorks Stacks console to specify custom JSON for a stack, on the stack settings page, type the custom JSON in the **Custom JSON** box, and then choose **Save**\.  
For more information, see [Using Custom JSON](workingstacks-json.md) and [Deploying Apps](workingapps-deploying.md)\.

**Instance shutdown timeout**  
Specifies how long \(in seconds\) AWS OpsWorks Stacks waits after triggering a [Shutdown lifecycle event](workingcookbook-events.md) before stopping or terminating the Amazon EC2 instance\. The default setting is 120 seconds\. The purpose of the setting is to give the instance's Shutdown recipes enough time to complete their tasks before terminating the instance\. If your custom Shutdown recipes might require more time, modify the setting accordingly\. For more information on instance shutdown, see [Stopping an Instance](workinginstances-starting.md#workinginstances-starting-stop)\.

The remaining settings on this tab vary with the type of layer and are identical to the settings on the layer's **Add Layer** page\.

## Recipes<a name="workinglayers-basics-edit-recipes"></a>

The **Recipes** tab includes the following settings\.

**Custom Chef recipes**  
You can assign custom Chef recipes to the layer's lifecycle events\. For more information, see [Executing Recipes](workingcookbook-executing.md)\.

## Network<a name="workinglayers-basics-edit-network"></a>

The **Network** tab includes the following settings\.

**Elastic Load Balancing**  
You can attach an Elastic Load Balancing load balancer to any layer\. AWS OpsWorks Stacks then automatically registers the layer's online instances with the load balancer and deregisters them when they go offline\. If you have enabled the load balancer's connection draining feature, you can specify whether AWS OpsWorks Stacks supports it\. For more information, see [Elastic Load Balancing Layer](layers-elb.md)\.

**Automatically Assign IP Addresses**  
You can control whether AWS OpsWorks Stacks automatically assigns public or Elastic IP addresses to the layer's instances\. Here's what happens when you enable this option:  
+ For instance store\-backed instances, AWS OpsWorks Stacks automatically assigns an address each time the instance is started\.
+ For Amazon EBS\-backed instances, AWS OpsWorks Stacks automatically assigns an address when the instance is started for the first time\.
+ If an instance belongs to more than one layer, AWS OpsWorks Stacks automatically assigns an address if you have enabled automatic assignment for at least one of the layers, 
If you enable automatic assignment of public IP addresses, it applies only to new instances\. AWS OpsWorks Stacks cannot update the public IP address for existing instances\.
If your stack is running in a VPC, you have separate settings for public and Elastic IP addresses\. The following table explains how these interact:  

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/ip-address.png)
Instances must have a way to communicate with the AWS OpsWorks Stacks service, Linux package repositories, and cookbook repositories\. If you specify no public or Elastic IP address, your VPC must include a component such as a NAT that allows the layer's instances to communicate with external sites\. For more information, see [Running a Stack in a VPC](workingstacks-vpc.md)\.
If your stack is not running in a VPC, **Elastic IP addresses** is your only setting:  
+ **Yes**: Instances receive an Elastic IP address when they are started for the first time, or a public IP address if an Elastic IP address cannot be assigned\.
+ **No**: Instances receive a public IP address each time they are started\.

## EBS Volumes<a name="workinglayers-basics-edit-ebs"></a>

The **EBS Volumes** tab includes the following settings\.

EBS optimized instances  
Whether the layer's instances should be optimized for Amazon Elastic Block Store \(Amazon EBS\)\. For more information, see [Amazon EBS\-Optimized Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html)\.

**Additional EBS Volumes**  
\(Linux only\) You can add [Amazon EBS volumes](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) to or remove them from the layer's instances\. When you start an instance, AWS OpsWorks Stacks automatically creates the volumes and attaches them to the instances\. You can use the **Resources** page to manage a stack's EBS volumes\. For more information, see [Resource Management](resources.md)\.  
+ **Mount point** – \(Required\) Specify the mount point or directory where the EBS volume will be mounted\.
+ **\# Disks** – \(Optional\) If you specified a RAID array, the number of disks in the array\.

  Each RAID level has a default number of disks, but you can select a larger number from the list\.
+ **Size total \(GiB\)** – \(Required\) The volume's size, in GiB\.

  For a RAID array, this setting specifies the total array size, not the size of each disk\.

  The following table shows the minimum and maximum volume sizes allowed for each volume type\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-basics-edit.html)
+ **Volume Type** – \(Optional\) Specify whether to create a magnetic, General Purpose SSD, Throughput Optimized HDD, Cold HDD, or PIOPS volume\.

  The default value is **Magnetic**\.
+ **Encrypted**– \(Optional\) Specify whether to encrypt the contents of the EBS volume\.
+ **IOPS per disk** – \(Required for Provisioned IOPS SSD and General Purpose SSD volumes\) If you specify a Provisioned IOPS SSD or General Purpose SSD volume, you must also specify the **IOPS per disk**\.

  For provisioned IOPS volumes, you can specify the IOPS rate when you create the volume\. The ratio of IOPS provisioned and the volume size requested can be a maximum of 30 \(in other words, a volume with 3000 IOPS must be at least 100 GB\)\. General Purpose \(SSD\) volume types have a baseline IOPS of volume size x 3 with a maximum of 10000 IOPS and can burst up to 3000 IOPS for 30 minutes\.

When you add volumes to or remove them from a layer, note the following:
+ If you add a volume, every new instance gets the new volume, but AWS OpsWorks Stacks does not update the existing instances\.
+ If you remove a volume, it applies only to new instances; the existing instances retain their volumes\.

### Specifying a Mount Point<a name="workinglayers-basics-edit-ebs-mount"></a>

You can specify any mount point that you prefer\. However, be aware that some mount points are reserved for use by AWS OpsWorks Stacks or Amazon EC2 and should not be used for Amazon EBS volumes\.

The following mount points are reserved for use by AWS OpsWorks Stacks\.
+ `/srv/www`
+ `/var/log/apache2` \(Ubuntu\)
+ `/var/log/httpd` \(Amazon Linux\)
+ `/var/log/mysql`
+ `/var/www` 

When an instance boots or reboots, autofs \(an automounting daemon\) uses ephemeral device mount points such as `/media/ephemeral0` for bind mounts\. This operation takes place before Amazon EBS volumes are mounted\. To ensure that your Amazon EBS volume's mount point does not conflict with autofs, do not specify an ephemeral device mount point\. The possible ephemeral device mount points depend on the particular instance type, and whether it is instance store–backed or Amazon EBS–backed\. To avoid a conflict with autofs, do the following:
+ Verify the ephemeral device mount points for the particular instance type and backing store that you want to use\.
+ Be aware that a mount point that works for an instance store–backed instance might conflict with autofs if you switch to an Amazon EBS–backed instance, or vice versa\.

**Note**  
If you want to change the instance store block device mapping, you can create a custom AMI\. For more information, see [Amazon EC2 Instance Store](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)\. For more information about how to create a custom AMI for AWS OpsWorks Stacks, see [Using Custom AMIs](workinginstances-custom-ami.md)\.

The following is an example of how to use a custom recipe to ensure that a volume's mount point doesn't conflict with autofs\. You can adapt it as needed for your particular use case\.

**To avoid a conflicting mount point**

1. Assign an Amazon EBS volume to desired layer but use a mount point such as `/mnt/workspace` that will never conflict with autofs\.

1. Implement the following custom recipe, which creates an application directory on the Amazon EBS volume and links to it from `/srv/www/`\. For more information on how to implement custom recipes, see [Cookbooks and Recipes](workingcookbook.md) and [Customizing AWS OpsWorks Stacks](customizing.md)\.

   ```
   mount_point = node['ebs']['raids']['/dev/md0']['mount_point'] rescue nil
   
   if mount_point
     node[:deploy].each do |application, deploy|
       directory "#{mount_point}/#{application}" do
         owner deploy[:user]
         group deploy[:group]
         mode 0770
         recursive true
       end
   
       link "/srv/www/#{application}" do
         to "#{mount_point}/#{application}"
       end
     end
   end
   ```

1. Add a `depends 'deploy'` line to the custom cookbook's `metadata.rb` file\.

1. [Assign this recipe to the layer's Setup event\.](workingcookbook-executing.md)

## Security<a name="workinglayers-basics-edit-security"></a>

The **Security** tab includes the following settings\.

**Security Groups**  
A layer must have at least one associated security group\. You specify how to associate security groups when you [create](workingstacks-creating.md) or [update](workingstacks-edit.md) a stack\. AWS OpsWorks Stacks provides a standard set of built\-in security groups\.   
+ The default option is to have AWS OpsWorks Stacks automatically associate the appropriate built\-in security group with each layer\.
+  You can also choose to not automatically associate built\-in security groups and instead associate a custom security group with each layer when you create the layer\.
For more information on security groups, see [Using Security Groups](workingsecurity-groups.md)\.  
After the layer has been created, you can use **Security Groups** to add more security groups to the layer by selecting them from the **Custom security groups** list\. After you add a security group to a layer, AWS OpsWorks Stacks adds it to all new instances\. \(Note that instance\-store instances that are restarted will be brought up as new instances, so they will also have the new security groups\.\) AWS OpsWorks Stacks does not add security groups to online instances\.  
You can delete existing security groups by clicking the **x**, as follows:  
+ If you chose to have AWS OpsWorks Stacks automatically associate built\-in security groups, you can delete custom security groups that you added earlier by clicking the **x**, but you cannot delete the built\-in group\.
+ If you chose to not automatically associate built\-in security groups, you can delete any existing security groups, including the original one, as long as the layer retains at least one group\.
After you remove a security group from a layer, AWS OpsWorks Stacks does not add it to any new or restarted instances\. AWS OpsWorks Stacks does not remove security groups from online instances\.  
If your stack is running in a VPC, including a [default VPC](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-supported-platforms.html), you can add or remove a security group for an online instance by using the Amazon EC2 console, API, or CLI\. However, this security group will not be visible in the AWS OpsWorks Stacks console\. If you want to remove the security group, you must also use Amazon EC2\. For more information, see [Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-network-security.html)\.
Note the following:  
+ You cannot restrict a built\-in security group's port access settings by adding a more restrictive security group\. When there are multiple security groups, Amazon EC2 uses the most permissive settings\.
+ You should not modify a built\-in security group's configuration\. When you create a stack, AWS OpsWorks Stacks overwrites the built\-in security groups' configurations, so any changes that you make will be lost the next time you create a stack\.
If you discover that you need more restrictive security group settings for one or more layers take these steps:  

1. Create custom security groups with appropriate settings and add them to the appropriate layers\.

   Every layer in your stack must have at least one security group in addition to the built\-in group, even if only one layer requires custom settings\.

1. [Edit the stack configuration](workingstacks-edit.md) and switch the **Use OpsWorks security groups** setting to **No**\.

   AWS OpsWorks Stacks automatically removes the built\-in security group from every layer\.
For more information on security groups, see [Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)\.

**EC2 Instance Profile**  
You can change the EC2 profile for the layer's instances\. For more information, see [Specifying Permissions for Apps Running on EC2 instances](opsworks-security-appsrole.md)\.

## CloudWatch Logs<a name="w4ab1c11c45c19b9c21"></a>

The **CloudWatch Logs** tab lets you enable or disable Amazon CloudWatch Logs\. CloudWatch Logs integration works with Chef 11\.10 and Chef 12 Linux\-based stacks\. For more information about enabling CloudWatch Logs integration and specifying the logs that you want to manage in the CloudWatch Logs console, see [Using Amazon CloudWatch Logs with AWS OpsWorks Stacks](monitoring-cloudwatch-logs.md)\.

## Tags<a name="w4ab1c11c45c19b9c23"></a>

The **Tags** tab lets you apply cost allocation tags to your layer\. After you add tags, you can activate them in the AWS Billing and Cost Management console\. When you create a tag, you are applying the tag to every resource within the tagged structure\. For example, if you apply a tag to a layer, you are applying the tag to every instance, Amazon EBS volume, or Elastic Load Balancing load balancer in the layer\. For more information about how to activate your tags and use them to track and manage the costs of your AWS OpsWorks Stacks resources, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) and [Activating User\-Defined Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html) in the *Billing and Cost Management User Guide*\. For more information about tagging in AWS OpsWorks Stacks, see [Tags](tagging.md)\.