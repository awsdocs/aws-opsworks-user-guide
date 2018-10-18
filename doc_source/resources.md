# Resource Management<a name="resources"></a>

The **Resources** page enables you to use your account's [Elastic IP address](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html), [Amazon EBS volume](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html), or [Amazon RDS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) instance resources in an AWS OpsWorks Stacks stack\. You can use **Resources** to do the following:
+ [Register a resource](resources-reg.md) with a stack, which allows you to attach the resource to one of the stack's instances\.
+ [Attach a resource](resources-attach.md) to one of the stack's instances\.
+ [Move a resource](resources-attach.md) from one instance to another\.
+ [Detach a resource](resources-detach.md) from an instance\. The resource remains registered and can be attached to another instance\.
+ [Deregister a resource](resources-dereg.md)\. An unregistered resource cannot be used by AWS OpsWorks Stacks, but it remains in your account unless you delete it, and can be registered with another stack\.

Note the following constraints:
+ You cannot attach registered Amazon EBS volumes to Windows instances\.
+ The Resources page manages standard, PIOPS, Throughput Optimized HDD, Cold HDD, or General Purpose \(SSD\) Amazon EBS volumes, but not RAID arrays\.
+ Amazon EBS volumes must be xfs formatted\.

  AWS OpsWorks Stacks does not support other file formats, such as ext4\. For more information on preparing Amazon EBS volumes, see [Making an Amazon EBS Volume Available for Use](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)\.
+ You can't attach an Amazon EBS volume to—or detach it from—a running instance\.

  You can operate only on offline instances\. For example, you can register an in\-use volume with a stack and attach it to an offline instance, but you must stop the original instance and detach the volume before starting the new instance\. Otherwise, the start process will fail\.
+ You can attach an Elastic IP address to and detach it from a running instance\.

  You can operate on online or offline instances\. For example, you can register an in\-use address and assign it to a running instance, and AWS OpsWorks Stacks will automatically reassign the address\.
+ To register and deregister resources, your IAM policy must grant permissions for the following actions:     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/resources.html)

  The [Manage permissions level](opsworks-security-users-console.md) grants permissions for all of these actions\. To prevent a Manage user from registering or deregistering particular resources, edit their IAM policy to deny permissions for the appropriate actions\. For more information, see [Security and Permissions](workingsecurity.md)\.

**Topics**
+ [Registering Resources with a Stack](resources-reg.md)
+ [Attaching and Moving Resources](resources-attach.md)
+ [Detaching Resources](resources-detach.md)
+ [Deregistering Resources](resources-dereg.md)