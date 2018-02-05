# Preparing the Instance<a name="registered-instances-register-registering-prepare"></a>

**Note**  
This feature is supported only for Linux stacks\.

Before registering an instance, you must ensure that it is compatible with AWS OpsWorks Stacks\. The details depend on whether you are registering an on\-premises or Amazon EC2 instance\.

## On\-Premises Instances<a name="registered-instances-register-prepare-onprem"></a>

An on\-premises instance must satisfy the following criteria:

+ The instance must run one of the supported Linux operating systems\. While it might be possible to create or register instances with other operating systems \(such as CentOS 6\.*x*\) that have been created from custom or community\-generated AMIs, these are not officially supported\.

  You must install the `libyaml` package on the instance\. For Ubuntu instances, the package is named `libyaml-0-2`\. For CentOS and Red Hat Enterprise Linux instances, the package is named `libyaml`\. 

+ The instance must have a supported instance type \(sometimes called the instance size\)\. Supported instance types can vary by operating system, and depend on whether your stack is in a VPC\. For a list of supported instance types, view the **Size** drop\-down list values that are shown in the AWS OpsWorks Stacks console when you try to create a new instance in your target stack\. If an instance type is grayed\-out, and cannot be created in your target stack, then you cannot register an instance of that type, either\.

+ The instance must have Internet access that allows it to communicate with the AWS OpsWorks Stacks service endpoint, `opsworks.us-east-1.amazonaws.com (HTTPS)`\. The instance must also support outbound connections to AWS resources such as Amazon S3\.

+ If you plan to register the instance from a separate workstation, the registered instance must support SSH login from the workstation\.

  SSH login is not required if you run the registration command from the instance\.

## Amazon EC2 Instances<a name="registered-instances-register-prepare-ec2"></a>

An Amazon EC2 instance must satisfy the following criteria:

+ The AMI must be based on one of the supported Linux operating systems\. For a current list, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.

  For more information, see [Using Custom AMIs](workinginstances-custom-ami.md)\.

  If the instance is based on a custom AMI that derives from a standard supported AMI, or if the instance contains a very minimal setup, you must install the `libyaml` package on the instance\. For Ubuntu instances, the package is named `libyaml-0-2`\. For Amazon Linux and Red Hat Enterprise Linux instances, the package is named `libyaml`\. 

+ The instance must have a supported instance type \(sometimes called the instance size\)\. Supported instance types can vary by operating system, and depend on whether your stack is in a VPC\. For a list of supported instance types, view the **Size** drop\-down list values that are shown in the AWS OpsWorks Stacks console when you try to create a new instance in your target stack\. If an instance type is grayed\-out, and cannot be created in your target stack, then you cannot register an instance of that type, either\.

+ The instance must be in the `running` state\.

+ The instance should not be part of an [Auto Scaling group](http://docs.aws.amazon.com/AutoScaling/latest/DeveloperGuide/WhatIsAutoScaling.html)\.

  For more information, see [Detach EC2 Instances From Your Auto Scaling Group](http://docs.aws.amazon.com/AutoScaling/latest/DeveloperGuide/detach-instance-asg.html)\.

+ The instance can be part of a [VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html), but it must be in the same VPC as the stack and the VPC must be configured to work properly with AWS OpsWorks Stacks\.

+ [Spot instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/how-spot-instances-work.html) are not supported, because they do not work with [auto healing](http://docs.aws.amazon.com/opsworks/latest/userguide/workinginstances-autohealing.html)\.

When you register an Amazon EC2 instance, AWS OpsWorks Stacks does not modify the instance's [security groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) or rules\. You must ensure that the instance's security group rules are compatible with AWS OpsWorks Stacks, as follows:

Ingress Rules  
The ingress rules should allow the following:  

+ SSH login\.

+ Traffic from the appropriate layers\.

  For example, a database server would typically allow inbound traffic from the stack's application server layers\.

+ Traffic to the appropriate ports\.

  For example, application server instances typically allow all inbound traffic to ports 80 \(HTTP\) and 443 \(HTTPS\)\.

Egress Rules  
The egress rules should allow the following:  

+ Traffic to the AWS OpsWorks Stacks service from applications running on the instance\. 

+ Traffic to access AWS resources such as Amazon S3 from applications using the AWS API\.
One common approach is to not specify any egress rules, which places no restrictions on outbound traffic\.