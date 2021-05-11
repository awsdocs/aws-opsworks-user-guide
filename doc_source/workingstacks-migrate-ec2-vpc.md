# Migrating stacks from Amazon EC2\-Classic to a VPC<a name="workingstacks-migrate-ec2-vpc"></a>

This topic describes how to migrate an AWS OpsWorks Stacks stack from the Amazon EC2 Classic network platform to an [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) network\.

If you created your AWS account before 2013\-12\-04, you might have support for EC2\-Classic in some AWS Regions\. Some Amazon EC2 resources and features, such as enhanced networking and newer instance types, require a virtual private cloud \(VPC\)\. Some resources can be shared between EC2\-Classic and a VPC, while some can't\. To avoid disruptions to your service, we recommend that you migrate your AWS OpsWorks Stacks stacks to a VPC\.

**Topics**
+ [Prerequisites](#workingstacks-migrate-vpc-prereqs)
+ [Migrate an AWS OpsWorks Stacks stack to a VPC](#workingstacks-migrate-vpc)
+ [See also](#workingstacks-migrate-seealso)

## Prerequisites<a name="workingstacks-migrate-vpc-prereqs"></a>

Before you begin, you must have a VPC that meets AWS OpsWorks Stacks configuration requirements\. To configure private subnets in your VPC for AWS OpsWorks Stacks, see [Running a Stack in a VPC](workingstacks-vpc.md) in this guide\. You can create a custom VPC by using the Amazon VPC management console\. For more information, see [Amazon VPC console wizard configurations](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html) and [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html) in the *Amazon Virtual Private Cloud User Guide*\.

To continue with migration, you'll need the VPC ID and the subnet ID that you want to use\.

## Migrate an AWS OpsWorks Stacks stack to a VPC<a name="workingstacks-migrate-vpc"></a>

First, clone an existing EC2\-Classic stack by using the AWS OpsWorks Stacks console or API\. Then, move the existing stack's resources to the new stack\. Start the new instances in the cloned stack, and deploy apps\. Verify that the new stack is working\. Finally, delete the EC2\-Classic resources from the EC2\-Classic stack, and then delete the old stack\.

1. Clone your existing EC2\-Classic stack into your VPC\. Cloning the stack copies stack settings, layers, apps, users, and user permissions to the new stack\. For more information about how to clone a stack, see [Clone a Stack](workingstacks-cloning.md) in this guide\.

   You can also clone a stack by using the AWS OpsWorks Stacks API\. When you clone a stack by using the AWS CLI or AWS SDKs, set the value of the `VpcId` parameter to the ID of the VPC that you created in [Prerequisites](#workingstacks-migrate-vpc-prereqs)\. For more information, see [https://docs.aws.amazon.com/opsworks/latest/APIReference/API_CloneStack.html](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_CloneStack.html) in the *AWS OpsWorks Stacks API Reference*\.

1. Create new instances in the layers of the cloned stack\. Be sure to specify the ID of the subnet you created in [Prerequisites](#workingstacks-migrate-vpc-prereqs)\. For more information about how to create instances in a stack, see [Adding an Instance to a Layer](workinginstances-add.md) in this guide\.

1. Migrate your classic resources, such as EC2 security groups, Elastic Load Balancing load balancers, and Elastic IP addresses to your VPC, and then associate them with the cloned stack\. For more information, see [Migrate your resources to a VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html#full-migrate) in the *Amazon EC2 User Guide*\.

1. Register Amazon EBS volumes and Amazon RDS instances with the cloned stack\. For more information about registering resources with a stack, see [Registering Resources with a Stack](resources-reg.md) in this guide\. 

   Amazon EBS volumes aren't associated with a VPC, and you can use them across instances in both EC2\-Classic stacks and stacks in a VPC\. You can register Amazon RDS instances in EC2\-Classic with both EC2\-Classic stacks and stacks in a VPC\.

1. Start instances in the cloned stack, and then move a small percentage of your workloads to the cloned stack\. For example, move a small percentage of traffic to the Elastic Load Balancing load balancers in the cloned stack\. If you are using Amazon Route 53, see [Routing traffic to an ELB load balancer](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html) in the *Amazon Route 53 Developer Guide*\.

   Route only a small percentage of traffic until you are sure that the new stack is functional and supports your applications\. Let the new stack work with a small percentage of traffic for a trial period, such as a week\. After you verify that the new stack is working, route remaining traffic to the stack\.

1. After you are sure the cloned stack is working, move the remainder of your production traﬃc or workloads to the cloned stack\. You can now stop instances in the EC2\-Classic stack\. We recommend that you keep the old stack available for several weeks, so you can move workloads back to the old stack if any issues occur with the new stack in the weeks after the migration\.

1. When the new stack has been working for several weeks, delete instances in the EC2\-Classic stack\. For more information about how to delete instances, see [Deleting AWS OpsWorks Stacks Instances](workinginstances-delete.md) in this guide\.
**Important**  
Do not use the Amazon EC2 console or API to stop or delete AWS OpsWorks instances\.

1. Delete apps in the EC2\-Classic stack\. For more information about how to delete apps, see [To delete the app from the stack](gettingstarted-intro-clean-up.md) in this guide\.

1. Delete the EC2\-Classic stack\. For more information about how to delete a stack, see [Delete a Stack](workingstacks-shutting.md) in this guide\.

## See also<a name="workingstacks-migrate-seealso"></a>
+ [Migrating from EC2\-Classic to a VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html#full-migrate)
+ [Debugging and Troubleshooting Guide](troubleshoot.md)
+ [Running a Stack in a VPC](workingstacks-vpc.md)