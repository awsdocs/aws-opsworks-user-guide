# Troubleshooting OpsWorks for Puppet Enterprise<a name="troubleshoot-opspup"></a>

This topic contains some common OpsWorks for Puppet Enterprise issues, and suggested solutions for those issues\.

**Topics**
+ [General troubleshooting tips](#w2ab1b7c41b7)
+ [Troubleshooting specific errors](#tshooterrors-puppet)
+ [Additional help and support](#tshooterrors-puppet-support)

## General troubleshooting tips<a name="w2ab1b7c41b7"></a>

If you are unable to create or work with a Puppet master, you can view error messages or logs to help you troubleshoot the issue\. The following tasks describe general places to start when you are troubleshooting a Puppet master issue\. For information about specific errors and solutions, see the [Troubleshooting specific errors](#tshooterrors-puppet) section of this topic\.
+ Use the OpsWorks for Puppet Enterprise console to view error messages if a Puppet master fails to start\. On the Puppet master properties page, error messages related to launching and running the server are shown at the top of the page\. Errors can come from OpsWorks for Puppet Enterprise, AWS CloudFormation, or Amazon EC2, services that are used to create a Puppet master\. On the properties page, you can also view events that occur on a running server, which can contain failure event messages\.
+ To help resolve EC2 issues, connect to your server's instance by using SSH, and view logs\. EC2 instance logs are stored in the `/var/log/aws/opsworks-cm` directory\. These logs capture command outputs while OpsWorks for Puppet Enterprise launches a Puppet master\.

## Troubleshooting specific errors<a name="tshooterrors-puppet"></a>

**Topics**
+ [Server is in a **Connection lost** state](#tshooterrors-puppet-connection-lost)
+ [Server creation fails with "requested configuration is currently not supported" message](#w2ab1b7c41b9b6)
+ [Unable to create the server's Amazon EC2 instance](#w2ab1b7c41b9b8)
+ [Service role error prevents server creation](#w2ab1b7c41b9c10)
+ [Elastic IP address limit exceeded](#w2ab1b7c41b9c12)
+ [Unattended node association fails](#w2ab1b7c41b9c14)
+ [System maintenance fails](#tshooterrors-puppet-maintenance-fails)

### Server is in a **Connection lost** state<a name="tshooterrors-puppet-connection-lost"></a>

**Problem:** A server's status shows as **Connection lost**\.

**Cause:** This most commonly occurs when an entity outside of AWS OpsWorks makes changes to an OpsWorks for Puppet Enterprise server or its supporting resources\. AWS OpsWorks cannot connect to Puppet Enterprise servers in **Connection lost** states to handle maintenance tasks such as creating backups, applying operating system patches, or updating Puppet\. As a result, your server might be missing important updates, susceptible to security issues, or otherwise not operating as expected\.

**Solution:** Try the following steps to restore the server's connection\.

1. Be sure that your service role has all required permissions\.

   1. On the **Settings** page for your server, in **Network and security**, choose the link for the service role that the server is using\. This opens the service role for viewing in the IAM console\.

   1. On the **Permissions** tab, verify that `AWSOpsWorksCMServiceRole` is in the **Permissions policies** list\. If it isn't listed, add the `AWSOpsWorksCMServiceRole` managed policy manually to the role\.

   1. On the **Trust relationships** tab, verify that the service role has a trust policy that trusts the `opsworks-cm.amazonaws.com` service to assume roles on your behalf\. For more information about how to use trust policies with roles, see [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html), or the AWS Security Blog post, [How to use trust policies with IAM roles](http://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)\.

1. Be sure that your instance profile has all required permissions\.

   1. On the **Settings** page for your server, in **Network and security**, choose the link for the instance profile that the server is using\. This opens the instance profile for viewing in the IAM console\.

   1. On the **Permissions** tab, verify that `AmazonEC2RoleforSSM` and `AWSOpsWorksCMInstanceProfileRole` are both in the **Permissions policies** list\. If one or both aren't listed, add these managed policies manually to the role\.

   1. On the **Trust relationships** tab, verify that the service role has a trust policy that trusts the `ec2.amazonaws.com` service to assume roles on your behalf\. For more information about how to use trust policies with roles, see [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html), or the AWS Security Blog post, [How to use trust policies with IAM roles](http://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)\.

1. In the Amazon EC2 console, be sure that you are in the same region as the region of the OpsWorks for Puppet Enterprise server, and then restart the EC2 instance that your server is using\.

   1. Choose the EC2 instance that is named `aws-opsworks-cm-instance-`*server\-name*\.

   1. On the **Instance state** menu, choose **Reboot instance**\.

   1. Allow up to 15 minutes for your server to restart and be fully online\.

1. In the OpsWorks for Puppet Enterprise console, on the server details page, verify that the server status is now **healthy**\.

If the server status is still **Connection lost** after performing the preceding steps, try one of the following\.
+ Replace the server by [creating a new one](gettingstarted-opspup-create.md) and [deleting the original](opspup-delete-server.md)\. If data on the current server is important to you, [restore the server from a recent backup](opspup-restore.md), and verify the data is up to date before [deleting the original, unresponsive server](opspup-delete-server.md)\.
+ [Contact AWS support](#tshooterrors-puppet-support)\.

### Server creation fails with "requested configuration is currently not supported" message<a name="w2ab1b7c41b9b6"></a>

**Problem:** You are trying to create a Puppet Enterprise server, but server creation fails with an error message that is similar to "The requested configuration is currently not supported\. Please check the documentation for supported configurations\."

**Cause:** An unsupported instance type might have been specified for the Puppet master\. If you choose to create the Puppet server in a VPC that has a non\-default tenancy, such as one for [dedicated instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html), all instances inside the specified VPC must also be of dedicated or host tenancy\. Because some instance types, such as t2, are available only with default tenancy, the Puppet master instance type might not be supportable by the specified VPC, and server creation fails\.

**Solution:** If you choose a VPC that has a non\-default tenancy, use an m4 instance type, which can support dedicated tenancy\.

### Unable to create the server's Amazon EC2 instance<a name="w2ab1b7c41b9b8"></a>

**Problem:** Server creation failed with an error message similar to the following: "The following resource\(s\) failed to create: \[EC2Instance\]\. Failed to receive 1 resource signal\(s\) within the specified duration\."

**Cause:** This is most likely because the EC2 instance doesn’t have network access\.

**Solution:** Ensure the instance has outbound Internet access, and the AWS service agent is able to issue commands\. Be sure that your VPC \(a VPC with a single public subnet\) has **DNS resolution** enabled, and that your subnet has the **Auto\-assign Public IP** setting enabled\.

### Service role error prevents server creation<a name="w2ab1b7c41b9c10"></a>

**Problem:** Server creation fails with an error message that states, "Not authorized to perform sts:AssumeRole\."

**Cause:** This can occur when the service role you are using lacks adequate permissions to create a new server\.

**Solution:** Open the OpsWorks for Puppet Enterprise console; use the console to generate a new service role and an instance profile role\. If you would prefer to use your own service role, attach the **AWSOpsWorksCMServiceRole** policy to the role\. Verify that **opsworks\-cm\.amazonaws\.com** is listed among services in the role's **Trust relationships**\. Verify that the service role that is associated with the Puppet master has the **AWSOpsWorksCMServiceRole** managed policy attached\.

### Elastic IP address limit exceeded<a name="w2ab1b7c41b9c12"></a>

**Problem:** Server creation fails with an error message that states, "The following resource\(s\) failed to create: \[EIP, EC2Instance\]\. Resource creation cancelled, the maximum number of addresses has been reached\."

**Cause:** This occurs when your account has used the maximum number of Elastic IP \(EIP\) addresses\. The default EIP address limit is five\.

**Solution:** You can either release existing EIP addresses or delete ones that your account is not actively using, or you can contact AWS Customer Support to increase the limit of EIP addresses that is associated with your account\.

### Unattended node association fails<a name="w2ab1b7c41b9c14"></a>

**Problem:** Unattended, or automatic, association of new Amazon EC2 nodes is failing\. Nodes that should have been added to the Puppet master are not showing up in the Puppet Enterprise dashboard\.

**Cause:** This can occur when you do not have an IAM role set up as an instance profile that permits `opsworks-cm` API calls to communicate with new EC2 instances\.

**Solution:** Attach a policy to your EC2 instance profile that allows the `AssociateNode` and `DescribeNodeAssociationStatus` API calls to work with EC2, as described in [Adding Nodes Automatically in OpsWorks for Puppet Enterprise](opspup-unattend-assoc.md)\.

### System maintenance fails<a name="tshooterrors-puppet-maintenance-fails"></a>

AWS OpsWorks CM performs weekly system maintenance to ensure that the latest AWS\-tested versions of Puppet Server, including security updates, are always running on an OpsWorks for Puppet Enterprise server\. If, for any reason, system maintenance fails, AWS OpsWorks CM notifies you of the failure\. For more information about system maintenance, see [System Maintenance in OpsWorks for Puppet Enterprise](opspup-maintenance.md)\.

This section describes possible reasons for failure and suggests solutions\.

**Topics**
+ [Service role or instance profile error prevents system maintenance](#w2ab1b7c41b9c16b8)

#### Service role or instance profile error prevents system maintenance<a name="w2ab1b7c41b9c16b8"></a>

**Problem:** System maintenance fails with an error message that states, "Not authorized to perform sts:AssumeRole", or a similar error message about permissions\.

**Cause:** This can occur when either the service role or instance profile you are using lacks adequate permissions to perform system maintenance on the server\.

**Solution:** Be sure that your service role and instance profile have all required permissions\. 

1. Be sure that your service role has all required permissions\.

   1. On the **Settings** page for your server, in **Network and security**, choose the link for the service role that the server is using\. This opens the service role for viewing in the IAM console\.

   1. On the **Permissions** tab, verify that `AWSOpsWorksCMServiceRole` is attached to the service role\. If `AWSOpsWorksCMServiceRole` is not listed, add this policy to the role\.

   1. Verify that **opsworks\-cm\.amazonaws\.com** is listed among services in the role's **Trust relationships**\. For more information about how to use trust policies with roles, see [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html), or the AWS Security Blog post, [How to use trust policies with IAM roles](http://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)\.

1. Be sure that your instance profile has all required permissions\.

   1. On the **Settings** page for your server, in **Network and security**, choose the link for the instance profile that the server is using\. This opens the instance profile for viewing in the IAM console\.

   1. On the **Permissions** tab, verify that `AmazonEC2RoleforSSM` and `AWSOpsWorksCMInstanceProfileRole` are both in the **Permissions policies** list\. If one or both aren't listed, add these managed policies manually to the role\.

   1. On the **Trust relationships** tab, verify that the service role has a trust policy that trusts the `ec2.amazonaws.com` service to assume roles on your behalf\. For more information about how to use trust policies with roles, see [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html), or the AWS Security Blog post, [How to use trust policies with IAM roles](http://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)\.

## Additional help and support<a name="tshooterrors-puppet-support"></a>

If you do not see your specific problem described in this topic, or you have tried the suggestions in this topic and are still having problems, visit the [AWS OpsWorks forums](https://forums.aws.amazon.com/forum.jspa?forumID=153&start=0)\.

You can also visit the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\. The AWS Support Center is the hub for creating and managing AWS Support cases\. The AWS Support Center also includes links to other helpful resources, such as forums, technical FAQs, service health status, and AWS Trusted Advisor\.