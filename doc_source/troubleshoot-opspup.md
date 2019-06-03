# Troubleshooting OpsWorks for Puppet Enterprise<a name="troubleshoot-opspup"></a>

This topic contains some common OpsWorks for Puppet Enterprise issues, and suggested solutions for those issues\.

**Topics**
+ [General Troubleshooting Tips](#w4ab1b7c35b7)
+ [Troubleshooting Specific Errors](#tshooterrors-puppet)
+ [Additional help and support](#w4ab1b7c35c11)

## General Troubleshooting Tips<a name="w4ab1b7c35b7"></a>

If you are unable to create or work with a Puppet master, you can view error messages or logs to help you troubleshoot the issue\. The following tasks describe general places to start when you are troubleshooting a Puppet master issue\. For information about specific errors and solutions, see the [Troubleshooting Specific Errors](#tshooterrors-puppet) section of this topic\.
+ Use the OpsWorks for Puppet Enterprise console to view error messages if a Puppet master fails to start\. On the Puppet master properties page, error messages related to launching and running the server are shown at the top of the page\. Errors can come from OpsWorks for Puppet Enterprise, AWS CloudFormation, or Amazon EC2, services that are used to create a Puppet master\. On the properties page, you can also view events that occur on a running server, which can contain failure event messages\.
+ To help resolve EC2 issues, connect to your server's instance by using SSH, and view logs\. EC2 instance logs are stored in the `/var/log/aws/opsworks-cm` directory\. These logs capture command outputs while OpsWorks for Puppet Enterprise launches a Puppet master\.

## Troubleshooting Specific Errors<a name="tshooterrors-puppet"></a>

**Topics**
+ [Server creation fails with "requested configuration is currently not supported" message](#w4ab1b7c35b9b4)
+ [Unable to create the server's Amazon EC2 instance](#w4ab1b7c35b9b6)
+ [Service role error prevents server creation](#w4ab1b7c35b9b8)
+ [Elastic IP address limit exceeded](#w4ab1b7c35b9c10)
+ [Unattended node association fails](#w4ab1b7c35b9c12)

### Server creation fails with "requested configuration is currently not supported" message<a name="w4ab1b7c35b9b4"></a>

**Problem:** You are trying to create a Puppet Enterprise server, but server creation fails with an error message that is similar to "The requested configuration is currently not supported\. Please check the documentation for supported configurations\."

**Cause:** An unsupported instance type might have been specified for the Puppet master\. If you choose to create the Puppet server in a VPC that has a non\-default tenancy, such as one for [dedicated instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html), all instances inside the specified VPC must also be of dedicated or host tenancy\. Because some instance types, such as t2, are available only with default tenancy, the Puppet master instance type might not be supportable by the specified VPC, and server creation fails\.

**Solution:** If you choose a VPC that has a non\-default tenancy, use an m4 instance type, which can support dedicated tenancy\.

### Unable to create the server's Amazon EC2 instance<a name="w4ab1b7c35b9b6"></a>

**Problem:** Server creation failed with an error message similar to the following: "The following resource\(s\) failed to create: \[EC2Instance\]\. Failed to receive 1 resource signal\(s\) within the specified duration\."

**Cause:** This is most likely because the EC2 instance doesn’t have network access\.

**Solution:** Ensure the instance has outbound Internet access, and the AWS service agent is able to issue commands\. Be sure that your VPC \(a VPC with a single public subnet\) has **DNS resolution** enabled, and that your subnet has the **Auto\-assign Public IP** setting enabled\.

### Service role error prevents server creation<a name="w4ab1b7c35b9b8"></a>

**Problem:** Server creation fails with an error message that states, "Not authorized to perform sts:AssumeRole\."

**Cause:** This can occur when the service role you are using lacks adequate permissions to create a new server\.

**Solution:** Open the OpsWorks for Puppet Enterprise console; use the console to generate a new service role and an instance profile role\. If you would prefer to use your own service role, attach the **AWSOpsWorksCMServiceRole** policy to the role\. Verify that **opsworks\-cm\.amazonaws\.com** is listed among services in the role's **Trust Relationships**\. Verify that the service role that is associated with the Puppet master has the **AWSOpsWorksCMServerRole** managed policy attached\.

### Elastic IP address limit exceeded<a name="w4ab1b7c35b9c10"></a>

**Problem:** Server creation fails with an error message that states, "The following resource\(s\) failed to create: \[EIP, EC2Instance\]\. Resource creation cancelled, the maximum number of addresses has been reached\."

**Cause:** This occurs when your account has used the maximum number of Elastic IP \(EIP\) addresses\. The default EIP address limit is five\.

**Solution:** You can either release existing EIP addresses or delete ones that your account is not actively using, or you can contact AWS Customer Support to increase the limit of EIP addresses that is associated with your account\.

### Unattended node association fails<a name="w4ab1b7c35b9c12"></a>

**Problem:** Unattended, or automatic, association of new Amazon EC2 nodes is failing\. Nodes that should have been added to the Puppet master are not showing up in the Puppet Enterprise dashboard\.

**Cause:** This can occur when you do not have an IAM role set up as an instance profile that permits `opsworks-cm` API calls to communicate with new EC2 instances\.

**Solution:** Attach a policy to your EC2 instance profile that allows the `AssociateNode` and `DescribeNodeAssociationStatus` API calls to work with EC2, as described in [Adding Nodes Automatically in OpsWorks for Puppet Enterprise](opspup-unattend-assoc.md)\.

## Additional help and support<a name="w4ab1b7c35c11"></a>

If you do not see your specific problem described in this topic, or you have tried the suggestions in this topic and are still having problems, visit the [AWS OpsWorks forums](https://forums.aws.amazon.com/forum.jspa?forumID=153&start=0)\.

You can also visit the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\. The AWS Support Center is the hub for creating and managing AWS Support cases\. The AWS Support Center also includes links to other helpful resources, such as forums, technical FAQs, service health status, and AWS Trusted Advisor\.