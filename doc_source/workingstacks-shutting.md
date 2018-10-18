# Delete a Stack<a name="workingstacks-shutting"></a>

If you no longer need a stack, you can delete it\. Only empty stacks can be deleted; you must first delete all instances, apps, and layers in the stack\.

**To delete a stack**

1. On the AWS OpsWorks Stacks dashboard, choose the stack that you want to shut down and delete\.

1. In the navigation pane, choose **Instances**\.

1. On the **Instances** page, choose **Stop all Instances**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/stop_all_instances.png)

1. After the instances have stopped, for each instance in the layer, choose **delete** in the **Actions** column\. When prompted to confirm, choose **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_instance.png)

1. When all the instances are deleted, in the navigation pane, choose **Layers**\.

1. On the **Layers** page, for each layer in the stack, choose **delete**\. On the confirmation prompt, choose **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_layer.png)

1. When all layers are deleted, in the navigation pane, choose **Apps**\.

1. On the **Apps** page, for each app in the stack, choose ** delete** in the **Actions** column\. On the confirmation prompt, choose **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_app.png)

1. When all apps are deleted, in the navigation pane, choose **Stack**\.

1. On the stack page, choose **Delete stack**\. On the confirmation prompt, choose **Yes, Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/delete_stack.png)

## Deleting Other AWS Resources Used by a Stack<a name="w4ab1c11c43c29b7"></a>

You use other AWS resources with AWS OpsWorks Stacks to create and manage your stacks\. As you delete a stack, consider also deleting resources that worked with with the stack, if another stack is not using them, and resources outside AWS OpsWorks Stacks are not using them\. The following are suggested reasons for cleaning up external AWS resources that you used with a stack\.
+ External AWS resources can continue to accrue charges on your AWS account\.
+ Resources such as Amazon S3 buckets can contain personally\-identifying, sensitive, or confidential information\.

**Important**  
Do not delete these resources if they are in use by other stacks\. Note that IAM roles and security groups are global, so stacks in other regions might be using these same resources\.

The following are other AWS resources that stacks use, and links to information about how to delete them\.

Service roles and instance profiles  
When you create a stack, you specify an IAM role and an instance profile that AWS OpsWorks Stacks uses to create allowed resources on your behalf\. AWS OpsWorks creates the role and instance profile for you if you do not choose existing ones\. The role and instance profile that AWS OpsWorks creates for you are named `aws-opsworks-service-role` and `aws-opsworks-ec2-role`, respectively\. If no other stacks in your account are using the IAM role and instance profile, it is safe to delete these resources\. For information about how to delete IAM roles and instance profiles, see [Deleting Roles or Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html) in the *IAM User Guide*\.

Security groups  
In AWS OpsWorks Stacks, you can specify user\-defined security groups at the layer level\. You create security groups by using the IAM console or API\. Stacks and layers in other regions can use the same security groups, because security groups are global\. You can delete a security group if it's not in use by other AWS resources\. For more information about how to delete a security group, see [Deleting an IAM Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_delete.html) in the *IAM User Guide*\.

Amazon EBS volumes  
In AWS OpsWorks Stacks, you add EBS volumes at the layer level, and they are attached to instances in the layer\. You create EBS volumes by using the Amazon EC2 service console or API, then attach them to AWS OpsWorks Stacks instances at the layer level\. EBS volumes are specific to an [availability zone](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html)\. If you are no longer using an EBS volume in any stacks in a specific region and availability zone, you can delete the volume\. For more information about how to delete an Amazon EBS volume, see [Deleting an Amazon EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-volume.html) in the *Amazon EC2 User Guide*\.

Amazon Simple Storage Service \(Amazon S3\) buckets  
In AWS OpsWorks Stacks, you can use Amazon S3 buckets for the following\. Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.  
+ Storing app code
+ Storing cookbooks and recipes
+ CloudTrail logs, if you have enabled CloudTrail logging in AWS OpsWorks Stacks
+ Amazon CloudWatch Logs streams, if you have enabled them in AWS OpsWorks Stacks

Elastic IP addresses  
If you [registered](https://docs.aws.amazon.com/opsworks/latest/userguide/resources-reg.html) [Elastic IP addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) with AWS OpsWorks Stacks, and you no longer need the Elastic IP addresses, you can [release the Elastic IP address](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-instance-addressing-eips-releasing)\.

Elastic Load Balancing load balancers  
If you no longer need an Elastic Load Balancing classic load balancer that you have been using with layers in your stack, you can delete it\. For more information, see [Delete Your Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-getting-started.html#delete-load-balancer) in the *User Guide for Classic Load Balancers*\.

Amazon Relational Database Service \(Amazon RDS\) instances  
If you [registered](https://docs.aws.amazon.com/opsworks/latest/userguide/resources-reg.html) Amazon RDS database \(DB\) instances with AWS OpsWorks Stacks, and you no longer need them, you can delete DB instances\. For more information about how to delete DB instances, see [Deleting a DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_DeleteInstance.html) in the *Amazon RDS User Guide*\.

Amazon Elastic Container Service \(Amazon ECS\) clusters  
If your stack included ECS cluster layers, and you are no longer using the ECS cluster that was registered with a layer, you can delete the ECS cluster\. For more information about how to delete an ECS cluster, see [Deleting a Cluster](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/delete_cluster.html) in the *Amazon ECS Developer Guide*\.