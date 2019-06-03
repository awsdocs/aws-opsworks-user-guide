# ECS Cluster Layers<a name="workinglayers-ecscluster"></a>

The [Amazon Elastic Container Service service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) \(Amazon ECS\) manages Docker containers on a cluster of Amazon Elastic Compute Cloud \(Amazon EC2\) instances, known as container instances\. An ECS Cluster layer represents an Amazon ECS cluster, and simplifies cluster management by providing features that include:
+ Streamlined container instance provisioning and management
+ Container instance operating system and package updates
+ User permissions management
+ Container instance performance monitoring
+ Amazon Elastic Block Store \(Amazon EBS\) volume management
+ Public and Elastic IP address management
+ Security group management

The ECS Cluster layer has the following restrictions and requirements:
+ The layer is available only for Chef 11\.10 or Chef 12 Linux stacks running in a VPC, including a [default VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html)\.
+ The layer's instances must be running one of the following operating systems\.
  + Amazon Linux 2
  + Amazon Linux 2018\.03
  + Amazon Linux 2017\.09
  + Amazon Linux 2017\.03
  + Amazon Linux 2016\.09
  + Amazon Linux 2016\.03
  + Amazon Linux 2015\.09
  + Amazon Linux 2015\.03
  + Ubuntu 18\.04 LTS
  + Ubuntu 16\.04 LTS
  + Ubuntu 14\.04 LTS
  + Custom
+ The [AWS OpsWorks Stacks agent version](workingstacks-creating.md#workingstacks-creating-advanced) on the layer's instances must be `3425-20150727112318` or later\.

**Topics**
+ [Adding an ECS Cluster Layer to a Stack](#workinglayers-ecscluster-add)
+ [Managing the ECS Cluster](#workinglayers-ecscluster-manage)
+ [Deleting an ECS Cluster Layer from a Stack](#workinglayers-ecscluster-delete)

## Adding an ECS Cluster Layer to a Stack<a name="workinglayers-ecscluster-add"></a>

AWS OpsWorks Stacks simplifies the process of launching and maintaining container instances for existing Amazon ECS clusters\. To create or launch other Amazon ECS entities, such as clusters and tasks, use the Amazon ECS console, command line interface \(CLI\), or API\. \(For more information, see the [Amazon Elastic Container Service Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/)\.\) You can then associate a cluster with a stack by creating an ECS Cluster layer, which you can use to manage the cluster in AWS OpsWorks Stacks\.

You can associate clusters with stacks as follows:
+ Each stack can have one ECS Cluster layer, which represents a single cluster\.
+ A cluster can be associated with only one stack\.

Before you can add ECS Cluster layers to your stacks, you must update the AWS OpsWorks Stacks AWS Identity and Access Management \(IAM\) service role, which is usually named `aws-opsworks-service-role`, to allow AWS OpsWorks Stacks to interact with Amazon ECS on your behalf\. For more information on the service role, see [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)\.

The first time you create an ECS Cluster layer, the console provides an **Update** button that you can choose to direct AWS OpsWorks Stacks to update the role for you\. AWS OpsWorks Stacks then displays the **Add Layer** page so you can add the layer to the stack\. You need to update the service role only once\. You can then use the updated role to add an ECS Cluster layer to any stack\.

**Note**  
If you prefer, you can manually update the service role's policy by adding `ecs:*` permission to the existing policy, as follows:  

```
{
  "Statement": [
    {
      "Action": [
        "ec2:*", 
        "iam:PassRole",
        "cloudwatch:GetMetricStatistics",
        "elasticloadbalancing:*",
        "rds:*",
        "ecs:*"
      ],
      "Effect": "Allow",
      "Resource": ["*"] 
    }
  ]
}
```

Associating a cluster with a stack requires two operations: registering the cluster with the stack and then creating the associated layer\. The AWS OpsWorks Stacks console combines these steps; layer creation automatically registers the specified cluster\. If you use the AWS OpsWorks Stacks API, CLI, or SDK, you must use separate operations to register the cluster and create the associated layer\. To use the console to add an ECS Cluster layer to your stack, choose **Layers**, choose **\+Layer** or **Add a Layer**, and then chose the ECS Cluster layer type\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/add_layer_ecs.png)

The **Add Layer** page includes the following configuration options:

**ECS Cluster**  
The Amazon ECS cluster that you want to register with the stack\. 

**EC2 Instance profile**  
The cluster's Amazon Elastic Compute Cloud\(Amazon EC2\) instance profile\. This profile grants permission for applications running on the cluster's container instances to access other AWS services, including Amazon ECS\. When you create your first ECS Cluster layer, choose **New profile with ECS access** to direct AWS OpsWorks Stacks to create the required profile, which is named `aws-opsworks-ec2-role-with-ecs`\. You can then use that profile for all subsequent ECS Cluster layers\. For more information on the instance profile, see [Specifying Permissions for Apps Running on EC2 instances](opsworks-security-appsrole.md)\.

You can specify other settings by [editing the layer's configuration](workinglayers-basics-edit.md), including:
+ [Attaching an Elastic Load Balancing load balancer](workinglayers-basics-edit.md#workinglayers-basics-edit-network) to the layer\.

  This approach might be suitable for some use cases, but Amazon ECS provides more sophisticated options\. For more information, see [Service Load Balancing](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-load-balancing.html)\.
+ Specifying whether to automatically [assign public IP addresses or Elastic IP addresses](workinglayers-basics-edit.md#workinglayers-basics-edit-network) to the container instances\.

  If you disable automatic assignment for both address types, the instance will not come online unless the subnet has a properly configured NAT\. For more information, see [Running a Stack in a VPC](workingstacks-vpc.md)\.

## Managing the ECS Cluster<a name="workinglayers-ecscluster-manage"></a>

After you create an ECS Cluster layer, you can use AWS OpsWorks Stacks to manage the cluster as follows:

**Provision and manage container instances**  
Initially, an ECS Cluster layer does not include any container instances, even if the original cluster did\. One option is to manage the layer's instances by using an appropriate combination of the following:  
+ Manually [add 24/7 instances](workinginstances-add.md) to the layer and [delete them](workinginstances-delete.md) when they are no longer needed\.
+ Add or delete instances on a schedule by adding [time\-based instances](workinginstances-autoscaling-timebased.md) to the layer\.
+ Add or delete instances based on AWS OpsWorks Stacks host metrics or CloudWatch alarms by adding [load\-based instances](workinginstances-autoscaling-loadbased.md) to the layer\.
If Amazon ECS is not supported for the stack's default operating system, you must explicitly specify a supported operating system—Amazon Linux 2, Amazon Linux 2018\.03, Amazon Linux 2017\.09, Amazon Linux 2017\.03, Amazon Linux 2016\.09, Amazon Linux 2016\.03, Amazon Linux 2015\.09, Amazon Linux 2015\.03, Ubuntu 18\.04 LTS, Ubuntu 16\.04 LTS, Ubuntu 14\.04 LTS, or Custom—when you create the container instances\. Do not use the ECS Optimized AMI to create instances in an ECS layer, because this AMI already includes the ECS agent\. AWS OpsWorks Stacks also attempts to install the ECS agent during the instance setup process, and the conflict can cause setup to fail\.
For more information, see [Optimizing the Number of Servers](best-practices-autoscale.md)\. AWS OpsWorks Stacks assigns the **AWS\-OpsWorks\-ECS\-Cluster** security group to each instance\. After each new instance finishes booting, AWS OpsWorks Stacks converts it into a container instance by installing Docker and the Amazon ECS agent, and then registering the instance with the cluster\.  
If you prefer to use existing container instances, you can [register them with the stack](registered-instances-register.md) and [assign them to the ECS Cluster layer](registered-instances-assign.md)\. Note that the instances must be running a supported operating system, Amazon Linux 2015\.03 or later, or Ubuntu 14\.04 LTS or later\.  
A container instance cannot belong to both an ECS Cluster layer and another built\-in layer\. However, a container instance *can* belong to an ECS Cluster layer and one or more [custom layers](workinglayers-custom.md)\.

**Run operating system and package updates**  
After a new instance finishes booting, AWS OpsWorks Stacks installs the latest updates\. You can then use AWS OpsWorks Stacks to keep the container instances up to date\. For more information, see [Managing Security Updates](workingsecurity-updates.md)\. 

**Manage user permissions**  
AWS OpsWorks Stacks provides a simple way to manage permissions on the container instances, including managing users' SSH keys\. For more information, see [Managing User Permissions](opsworks-security-users.md) and [Managing SSH Access](security-ssh-access.md)\.

**Monitor performance metrics**  
AWS OpsWorks Stacks provides a variety of ways to monitor performance metrics for the stack, layer, or individual instances\. For more information, see [Monitoring](monitoring.md)\.

You handle other management tasks, such as creating tasks or services, through Amazon ECS\. For more information, see the [Amazon Elastic Container Service Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/)\.

**Note**  
To go directly to the cluster's page on the Amazon ECS console, choose **Instances**, and then choose **ECS Cluster**, which is near the upper right corner of the ECS Cluster layer's section\.

## Deleting an ECS Cluster Layer from a Stack<a name="workinglayers-ecscluster-delete"></a>

When you no longer need the cluster, delete the ECS Cluster layer and deregister the associated cluster\. Removing a cluster from a stack requires two operations: deregistering the cluster and then deleting the associated layer\. The AWS OpsWorks Stacks console combines these steps; layer deletion automatically deregisters the specified cluster\. If you use the AWS OpsWorks Stacks API, CLI, or SDK, you must use separate operations to deregister the cluster and delete the associated layer\.

**To use the console to delete an ECS Cluster layer**

1. If you want to control how tasks are shut down, use the Amazon ECS console, API, or CLI to scale down and delete the cluster's services\. For more information, see [Cleaning Up Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CleaningUp.html)\.

1. [Stop the layer's instances](workinginstances-starting.md#workinginstances-starting-stop), and then [delete them](workinginstances-delete.md)\. When you stop a container instance, AWS OpsWorks Stacks automatically stops any running tasks, deregisters the instance from the cluster, and terminates the instance\.
**Note**  
If you have registered existing container instances with the stack, you can [unassign the instances from the layer](registered-instances-unassign.md) and then [deregister them](registered-instances-deregister.md), which returns the instances to ECS control\.

1. [Delete the layer](workinglayers-basics-delete.md)\. AWS OpsWorks Stacks deregisters the associated cluster, but does not delete it\. The cluster remains in Amazon ECS\. 