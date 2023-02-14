# Troubleshooting AWS OpsWorks for Chef Automate<a name="troubleshoot-opscm"></a>

This topic contains some common AWS OpsWorks for Chef Automate issues, and suggested solutions for those issues\.

**Topics**
+ [General troubleshooting tips](#w2ab1b9c52b7)
+ [Troubleshooting specific errors](#tshooterrors-chef)
+ [Additional help and support](#tshooterrors-chef-support)

## General troubleshooting tips<a name="w2ab1b9c52b7"></a>

If you are unable to create or work with a Chef server, you can view error messages or logs to help you troubleshoot the issue\. The following tasks describe general places to start when you are troubleshooting a Chef server issue\. For information about specific errors and solutions, see the [Troubleshooting specific errors](#tshooterrors-chef) section of this topic\.
+ Use the AWS OpsWorks for Chef Automate console to view error messages if a Chef server fails to start\. On the Chef server detail page, error messages related to launching and running the server are shown at the top of the page\. Errors can come from AWS OpsWorks for Chef Automate, AWS CloudFormation, or Amazon EC2, services that are used to create a Chef server\. On the detail page, you can also view events that occur on a running server, which can contain failure event messages\.
+ To help resolve EC2 issues, connect to your server's instance by using SSH, and view logs\. EC2 instance logs are stored in the `/var/log/aws/opsworks-cm` directory\. These logs capture command outputs while AWS OpsWorks for Chef Automate launches a Chef server\.

## Troubleshooting specific errors<a name="tshooterrors-chef"></a>

**Topics**
+ [Server is in a **Connection lost** state](#tshooterrors-chef-connection-lost)
+ [Managed node shows up in the Chef Automate dashboard in the Missing column](#w2ab1b9c52b9b6)
+ [Cannot create a Chef vault; `knife vault` command fails with errors](#w2ab1b9c52b9b8)
+ [Server creation fails with "requested configuration is currently not supported" message](#w2ab1b9c52b9c10)
+ [Chef server doesn't recognize organization names added in the Chef Automate dashboard](#w2ab1b9c52b9c12)
+ [Unable to create the server's Amazon EC2 instance](#w2ab1b9c52b9c14)
+ [Service role error prevents server creation](#w2ab1b9c52b9c16)
+ [Elastic IP address limit exceeded](#w2ab1b9c52b9c18)
+ [Cannot sign into the Chef Automate dashboard](#w2ab1b9c52b9c20)
+ [Unattended node association fails](#w2ab1b9c52b9c22)
+ [System maintenance fails](#tshooterrors-chef-maintenance-fails)

### Server is in a **Connection lost** state<a name="tshooterrors-chef-connection-lost"></a>

**Problem:** A server's status shows as **Connection lost**\.

**Cause:** This most commonly occurs when an entity outside of AWS OpsWorks makes changes to an AWS OpsWorks for Chef Automate server or its supporting resources\. AWS OpsWorks cannot connect to Chef Automate servers in **Connection lost** states to handle maintenance tasks such as creating backups, applying operating system patches, or updating Chef Automate\. As a result, your server might be missing important updates, susceptible to security issues, or otherwise not operating as expected\.

**Solution:** Try the following steps to restore the server's connection\.

1. Be sure that your service role has all required permissions\.

   1. On the **Settings** page for your server, in **Network and security**, choose the link for the service role that the server is using\. This opens the service role for viewing in the IAM console\.

   1. On the **Permissions** tab, verify that `AWSOpsWorksCMServiceRole` is in the **Permissions policies** list\. If it isn't listed, add the `AWSOpsWorksCMServiceRole` managed policy manually to the role\.

   1. On the **Trust relationships** tab, verify that the service role has a trust policy that trusts the `opsworks-cm.amazonaws.com` service to assume roles on your behalf\. For more information about how to use trust policies with roles, see [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html), or the AWS Security Blog post, [How to use trust policies with IAM roles](http://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)\.

1. Be sure that your instance profile has all required permissions\.

   1. On the **Settings** page for your server, in **Network and security**, choose the link for the instance profile that the server is using\. This opens the instance profile for viewing in the IAM console\.

   1. On the **Permissions** tab, verify that `AmazonEC2RoleforSSM` and `AWSOpsWorksCMInstanceProfileRole` are both in the **Permissions policies** list\. If one or both aren't listed, add these managed policies manually to the role\.

   1. On the **Trust relationships** tab, verify that the service role has a trust policy that trusts the `ec2.amazonaws.com` service to assume roles on your behalf\. For more information about how to use trust policies with roles, see [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html), or the AWS Security Blog post, [How to use trust policies with IAM roles](http://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)\.

1. In the Amazon EC2 console, be sure that you are in the same region as the region of the AWS OpsWorks for Chef Automate server, and then restart the EC2 instance that your server is using\.

   1. Choose the EC2 instance that is named `aws-opsworks-cm-instance-`*server\-name*\.

   1. On the **Instance state** menu, choose **Reboot instance**\.

   1. Allow up to 15 minutes for your server to restart and be fully online\.

1. In the AWS OpsWorks for Chef Automate console, on the server details page, verify that the server status is now **healthy**\.

If the server status is still **Connection lost** after performing the preceding steps, try one of the following\.
+ Replace the server by [creating a new one](gettingstarted-opscm-create.md) and [deleting the original](opscm-delete-server.md)\. If data on the current server is important to you, [restore the server from a recent backup](opscm-chef-restore.md), and verify the data is up to date before [deleting the original, unresponsive server](opscm-delete-server.md)\.
+ [Contact AWS support](#tshooterrors-chef-support)\.

### Managed node shows up in the Chef Automate dashboard in the Missing column<a name="w2ab1b9c52b9b6"></a>

**Problem:** A managed node is showing up in the Chef Automate dashboard's **Missing** column\.

**Cause:** When a node doesn't connect to the Chef Automate server for more than 12 hours, and `chef-client` cannot run on the node, the node changes from the state it was in before the 12\-hour period, and moves to the **Missing** column of the Chef Automate dashboard\.

**Solution:** Verify that the node is online\. Try running `knife node show node_name --run-list` to see whether `chef-client` is able to run on the node, or `knife node show -l node_name` to display all information about the node\. The node might be offline or disconnected from the network\.

### Cannot create a Chef vault; `knife vault` command fails with errors<a name="w2ab1b9c52b9b8"></a>

**Problem:** You are trying to create a vault on your Chef Automate server \(such as a vault for storing credentials for domain\-joining Windows\-based nodes\) by running the `knife vault` command\. The command returns an error message similar to the following\.

```
WARN: Auto inflation of JSON data is deprecated. Please pass in the class to inflate or use #edit_hash (CHEF-1) 
at /opt/chefdk/embedded/lib/ruby/2.3.0/forwardable.rb:189:in `edit_data'.Please see https://docs.chef.io/deprecations_json_auto_inflate.html 
for further details and information on how to correct this problem.
WARNING: pivotal not found in users, trying clients.
ERROR: ChefVault::Exceptions::AdminNotFound: FATAL: Could not find pivotal in users or clients!
```

The pivotal user is not returned when you run `knife user list` remotely, but you can see the pivotal user in results when you run the `chef-server-ctl user-show` command locally on your Chef Automate server\. In other words, your `knife vault` command cannot find the pivotal user, but you know it exists\.

**Cause:** Though the pivotal user is considered the superuser in Chef, and has full permissions, it is not a member of any organization, including the `default` organization that is used in AWS OpsWorks for Chef Automate\. The command `knife user list` returns all the users that are in the current organization in your Chef configuration\. The `chef-server-ctl user-show` command returns all users regardless of organization, including the pivotal user\.

**Solution:** To fix the problem, add the pivotal user to the default organization by running `knife opc`\.

First, you'll need to install the [knife\-opc](https://github.com/chef/knife-opc) plugin\.

```
chef gem install knife-opc
```

After you install the plugin, run the following command to add the pivotal user to the default organization\.

```
knife opc org user add default pivotal
```

You can verify that the pivotal user is part of the default organization by running `knife user list` again\. `pivotal` should be listed in the results\. Then, try running `knife vault` again\.

### Server creation fails with "requested configuration is currently not supported" message<a name="w2ab1b9c52b9c10"></a>

**Problem:** You are trying to create a Chef Automate server, but server creation fails with an error message that is similar to "The requested configuration is currently not supported\. Please check the documentation for supported configurations\."

**Cause:** An unsupported instance type might have been specified for the Chef Automate server\. If you choose to create the Chef Automate server in a VPC that has a non\-default tenancy, such as one for [dedicated instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html), all instances inside the specified VPC must also be of dedicated or host tenancy\. Because some instance types, such as t2, are available only with default tenancy, the Chef Automate server instance type might not be supportable by the specified VPC, and server creation fails\.

**Solution:** If you choose a VPC that has a non\-default tenancy, use an m4 instance type, which can support dedicated tenancy\.

### Chef server doesn't recognize organization names added in the Chef Automate dashboard<a name="w2ab1b9c52b9c12"></a>

**Problem:** You've added new Workflow organization names in the Chef Automate dashboard, or specified a `CHEF_AUTOMATE_ORGANIZATION` value other than `"default"` in the [unattended node association script](opscm-unattend-assoc.md), but node association fails\. Your AWS OpsWorks for Chef Automate server does not recognize the new organization names\.

**Cause:** Workflow organization names and Chef server organization names are not the same\. You can create new Workflow organizations in the web\-based Chef Automate dashboard, but not Chef server organization names\. You can use the Chef Automate dashboard only to view existing Chef server organizations\. A new organization that you create in the Chef Automate dashboard is a Workflow organization, and is not recognized by the Chef server\. You cannot create new organization names by specifying them in the node association script\. Referring to an organization name in a node association script when the organization has not first been added to the Chef server will cause node association to fail\.

**Solution:** To create new organizations that are recognized on the Chef server, use the [https://docs.chef.io/plugin_knife_opc.html#opc-org-create](https://docs.chef.io/plugin_knife_opc.html#opc-org-create) command, or run [https://docs.chef.io/ctl_chef_server.html#organization-management](https://docs.chef.io/ctl_chef_server.html#organization-management)\.

### Unable to create the server's Amazon EC2 instance<a name="w2ab1b9c52b9c14"></a>

**Problem:** Server creation failed with an error message similar to the following: "The following resource\(s\) failed to create: \[EC2Instance\]\. Failed to receive 1 resource signal\(s\) within the specified duration\."

**Cause:** This is most likely because the EC2 instance doesn’t have network access\.

**Solution:** Ensure the instance has outbound Internet access, and the AWS service agent is able to issue commands\. Be sure that your VPC \(a VPC with a single public subnet\) has **DNS resolution** enabled, and that your subnet has the **Auto\-assign Public IP** setting enabled\.

### Service role error prevents server creation<a name="w2ab1b9c52b9c16"></a>

**Problem:** Server creation fails with an error message that states, "Not authorized to perform sts:AssumeRole\."

**Cause:** This can occur when the service role you are using lacks adequate permissions to create a new server\.

**Solution:** Open the AWS OpsWorks for Chef Automate console; use the console to generate a new service role and an instance profile role\. If you would prefer to use your own service role, attach the **AWSOpsWorksCMServiceRole** policy to the role\. Verify that **opsworks\-cm\.amazonaws\.com** is listed among services in the role's **Trust relationships**\. Verify that the service role that is associated with the Chef server has the **AWSOpsWorksCMServiceRole** managed policy attached\.

### Elastic IP address limit exceeded<a name="w2ab1b9c52b9c18"></a>

**Problem:** Server creation fails with an error message that states, "The following resource\(s\) failed to create: \[EIP, EC2Instance\]\. Resource creation cancelled, the maximum number of addresses has been reached\."

**Cause:** This occurs when your account has used the maximum number of Elastic IP \(EIP\) addresses\. The default EIP address limit is five\.

**Solution:** You can either release existing EIP addresses or delete ones that your account is not actively using, or you can contact AWS Customer Support to increase the limit of EIP addresses that is associated with your account\.

### Cannot sign into the Chef Automate dashboard<a name="w2ab1b9c52b9c20"></a>

**Problem:** The Chef Automate dashboard shows an error similar to the following: "Cross\-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://myserver\-name\.region\.opsworks\-cm\.io/api/v0/e/default/verify\-token\. \(Reason: CORS header 'Access\-Control\-Allow\-Origin' missing\)"\. The error can also be similar to "The User Id / Password combination entered is incorrect\."

**Cause:** The Chef Automate dashboard explicity sets the FQDN, and does not accept relative URLs\. At this time, you cannot sign in by using the Chef server's IP address; you can only sign in by using the DNS name of the server\.

**Solution:** Sign in to the Chef Automate dashboard only by using the Chef server's DNS name entry, not its IP address\. You can also try resetting the Chef Automate dashboard credentials by running an AWS CLI command, as described in [Reset Chef Automate Dashboard Credentials](opscm-resetchefcreds.md)\.

### Unattended node association fails<a name="w2ab1b9c52b9c22"></a>

**Problem:** Unattended, or automatic, association of new Amazon EC2 nodes is failing\. Nodes that should have been added to the Chef server are not showing up in the Chef Automate dashboard, and are not listed in results of the `knife client show` or `knife node show` commands\.

**Cause:** This can occur when you do not have an IAM role set up as an instance profile that permits `opsworks-cm` API calls to communicate with new EC2 instances\.

**Solution:** Attach a policy to your EC2 instance profile that allows the `AssociateNode` and `DescribeNodeAssociationStatus` API calls to work with EC2, as described in [Adding Nodes Automatically in AWS OpsWorks for Chef Automate](opscm-unattend-assoc.md)\.

### System maintenance fails<a name="tshooterrors-chef-maintenance-fails"></a>

AWS OpsWorks CM performs weekly system maintenance to ensure that the latest minor versions of Chef Server and Chef Automate Server, including security updates, are always running on an AWS OpsWorks for Chef Automate server\. If, for any reason, system maintenance fails, AWS OpsWorks CM notifies you of the failure\. For more information about system maintenance, see [System Maintenance in AWS OpsWorks for Chef Automate](opscm-maintenance.md)\.

This section describes possible reasons for failure and suggests solutions\.

**Topics**
+ [Service role or instance profile error prevents system maintenance](#w2ab1b9c52b9c24b8)

#### Service role or instance profile error prevents system maintenance<a name="w2ab1b9c52b9c24b8"></a>

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

## Additional help and support<a name="tshooterrors-chef-support"></a>

If you do not see your specific problem described in this topic, or you have tried the suggestions in this topic and are still having problems, visit the [AWS OpsWorks forums](https://forums.aws.amazon.com/forum.jspa?forumID=153&start=0)\.

You can also visit the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\. The AWS Support Center is the hub for creating and managing AWS Support cases\. The AWS Support Center also includes links to other helpful resources, such as forums, technical FAQs, service health status, and AWS Trusted Advisor\.