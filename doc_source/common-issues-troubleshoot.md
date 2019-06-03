# Troubleshooting AWS OpsWorks Stacks<a name="common-issues-troubleshoot"></a>

This section contains some commonly encountered AWS OpsWorks Stacks issues and their solutions\.

**Topics**
+ [Unable to Manage Instances](#w4ab1c11c69c15b7b7)
+ [After a Chef Run, Instances Won't Boot](#w4ab1c11c69c15b7b9)
+ [A Layer's Instances All Fail an Elastic Load Balancing Health Check](#common-issues-troubleshoot-health)
+ [Can't Communicate with an Elastic Load Balancing Load Balancer](#w4ab1c11c69c15b7c13)
+ [An Imported On\-premises Instance Fails to Finish Volume Setup After a Restart](#w4ab1c11c69c15b7c15)
+ [An EBS Volume Does Not Reattach After a Reboot](#common-issues-troubleshoot-ebs)
+ [Can't Delete AWS OpsWorks Stacks Security Groups](#common-issues-troubleshoot-booting-secgroup)
+ [Accidentally Deleted an AWS OpsWorks Stacks Security Group](#common-issues-troubleshoot-booting-secgroup-delete)
+ [Chef Log Terminates Abruptly](#common-issues-troubleshoot-log-terminates)
+ [Cookbook Does Not Get Updated](#common-issues-troubleshoot-update)
+ [Instances are Stuck at Booting Status](#common-issues-troubleshoot-booting)
+ [Instances Unexpectedly Restart](#common-issues-troubleshoot-restart)
+ [`opsworks-agent` Processes are Running on Instances](#common-issues-troubleshoot-agent)
+ [Unexpected execute\_recipes Commands](#common-issues-troubleshoot-unexpected)

## Unable to Manage Instances<a name="w4ab1c11c69c15b7b7"></a>

**Problem:** You are no longer able to manage an instance that has been manageable in the past\. In some cases, logs can show an error similar to the following\.

```
Aws::CharlieInstanceService::Errors::UnrecognizedClientException - The security token included in the request is invalid.
```

**Cause:** This can occur if a resource outside AWS OpsWorks on which the instance depends was edited or deleted\. The following are examples of resource changes that can break communications with an instance\.
+ An IAM user or role associated with the instance has been deleted accidentally, outside of AWS OpsWorks Stacks\. This causes a communication failure between the AWS OpsWorks agent that is installed on the instance, and the AWS OpsWorks Stacks service\. The IAM user that is associated with an instance is required throughout the life of the instance\.
+ Editing volume or storage configurations while an instance is offline can make an instance unmanageable\.
+ Adding EC2 instances to an ELB manually\. AWS OpsWorks reconfigures an assigned Elastic Load Balancing load balancer each time an instance enters or leaves the online state\. AWS OpsWorks only considers instances it knows about to be valid members; instances that are added outside of AWS OpsWorks, or by some other process, are removed\. Every other instance is removed\.

**Solution:** Do not delete IAM users or roles upon which your instances depend\. If possible, edit volume or storage configurations only while dependent instances are running\. Use AWS OpsWorks to manage the load balancer or EIP memberships of AWS OpsWorks instances\. When you are registering an instance, to help prevent problems managing registered instances in the event that the IAM user is accidentally deleted, add the `--use-instance-profile` parameter to your `register` command to use the instance's built\-in instance profile instead\.

## After a Chef Run, Instances Won't Boot<a name="w4ab1c11c69c15b7b9"></a>

**Problem:** On Chef 11\.10 or older stacks that are configured to use custom cookbooks, after a Chef run that used community cookbooks, instances won't boot\. Log messages can state that recipes failed to compile \("Recipe Compile Error"\), or can't be loaded because they are unable to find a dependency\.

**Cause:** The most likely cause is that the custom or community cookbook does not support the Chef version that your stack uses\. Some popular community cookbooks, such as `[apt](https://supermarket.chef.io/cookbooks/apt)` and `[build\-essential](https://supermarket.chef.io/cookbooks/build-essential/versions/3.2.0)`, have known compatibility issues with Chef 11\.10\.

**Solution:** On AWS OpsWorks Stacks stacks that have the **Use custom Chef cookbooks** setting turned on, custom or community cookbooks must always support the version of Chef that your stack uses\. Pin community cookbooks to a version \(that is, set the cookbook version number to a specific version\) that is compatible with the version of Chef that is configured in your stack settings\. To find a supported community cookbook version, view the changelog for a cookbook that fails to compile, and use only the most current version of the cookbook that your stack will support\. To pin a cookbook version, specify an exact version number in your custom cookbook repository's Berksfile\. For example, `cookbook 'build-essential', '= 3.2.0'`\.

## A Layer's Instances All Fail an Elastic Load Balancing Health Check<a name="common-issues-troubleshoot-health"></a>

**Problem:** You attach an Elastic Load Balancing load balancer to an app server layer, but all the instances fail the health check\.

**Cause:** When you create an Elastic Load Balancing load balancer, you must specify the ping path that the load balancer calls to determine whether the instance is healthy\. Be sure to specify a ping path that is appropriate for your application; the default value is /index\.html\. If your application does not include an `index.html`, you must specify an appropriate path or the health check will fail\. For example, the SimplePHPApp application used in [Getting Started with Chef 11 Linux Stacks](gettingstarted.md) does not use index\.html; the appropriate ping path for those servers is /\. 

**Solution:** Edit the load balancer's ping path\. For more information, see [Elastic Load Balancing](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/gs-ec2classic.html)

## Can't Communicate with an Elastic Load Balancing Load Balancer<a name="w4ab1c11c69c15b7c13"></a>

**Problem:** You create an Elastic Load Balancing load balancer and attach it to an app server layer, but when you click the load balancer's DNS name or IP address to run the application, you get the following error: "The remote server is not responding"\.

**Cause:** If your stack is running in a default VPC, when you create an Elastic Load Balancing load balancer in the region, you must specify a security group\. The security group must have ingress rules that allow inbound traffic from your IP address\. If you specify **default VPC security group**, the default ingress rule does not accept any inbound traffic\. 

**Solution:** Edit the security group ingress rules to accept inbound traffic from appropriate IP addresses\.

1. Click **Security Groups** in the [Amazon EC2 console's](https://console.aws.amazon.com/ec2/) navigation pane\.

1. Select the load balancer's security group\.

1. Click **Edit** on the **Inbound** tab\.

1. Add an ingress rule with **Source** set to an appropriate CIDR\.

   For example, specifying **Anywhere** sets the CIDR to 0\.0\.0\.0/0, which directs the load balancer to accept incoming traffic from any IP address\. 

## An Imported On\-premises Instance Fails to Finish Volume Setup After a Restart<a name="w4ab1c11c69c15b7c15"></a>

**Problem:** You restart an EC2 instance that you have imported into AWS OpsWorks Stacks, and the AWS OpsWorks Stacks console displays **failed** as the instance status\. This can occur on either Chef 11 or Chef 12 instances\.

**Cause: **AWS OpsWorks Stacks might be unable to attach a volume to your instance during the setup process\. One possible cause is that AWS OpsWorks Stacks overwrites your volume configuration on your instance when you run the `setup` command\.

**Solution:** Open the **Details** page for the instance, and check your volume configuration in the **Volumes** area\. Note that you can change your volume configuration only when your instance is in the **stopped** state\. Be sure that every volume has a specified mount point and a name\. Confirm that you provided the correct mount point in your configuration in AWS OpsWorks Stacks before you restart the instance\.

## An EBS Volume Does Not Reattach After a Reboot<a name="common-issues-troubleshoot-ebs"></a>

**Problem:**You use the Amazon EC2 console to attach an Amazon EBS volume to an instance but when you reboot the instance, the volume is no longer attached\. 

**Cause:** AWS OpsWorks Stacks can reattach only those Amazon EBS volumes that it is aware of, which are limited to the following:
+ Volumes that were created by AWS OpsWorks Stacks\.
+ Volumes from your account that you have explicitly registered with a stack by using the **Resources** page\. 

**Solution:** Manage your Amazon EBS volumes only by using the AWS OpsWorks Stacks console, API, or CLI\. If you want to use one of your account's Amazon EBS volumes with a stack, use the stack's **Resources** page to register the volume and attach it to an instance\. For more information, see [Resource Management](resources.md)\.

## Can't Delete AWS OpsWorks Stacks Security Groups<a name="common-issues-troubleshoot-booting-secgroup"></a>

**Problem:** After you delete a stack, there are a number of AWS OpsWorks Stacks security groups left behind that can't be deleted\.

**Cause:** The security groups must be deleted in a particular order\.

**Solution:** First, make sure that no instances are using the security groups\. Then, delete any of the following security groups, if they exist, in the following order: 

1. AWS\-OpsWorks\-Blank\-Server

1. AWS\-OpsWorks\-Monitoring\-Master\-Server

1. AWS\-OpsWorks\-DB\-Master\-Server

1. AWS\-OpsWorks\-Memcached\-Server

1. AWS\-OpsWorks\-Custom\-Server

1. AWS\-OpsWorks\-nodejs\-App\-Server

1. AWS\-OpsWorks\-PHP\-App\-Server

1. AWS\-OpsWorks\-Rails\-App\-Server

1. AWS\-OpsWorks\-Web\-Server

1. AWS\-OpsWorks\-Default\-Server

1. AWS\-OpsWorks\-LB\-Server

## Accidentally Deleted an AWS OpsWorks Stacks Security Group<a name="common-issues-troubleshoot-booting-secgroup-delete"></a>

**Problem:** You deleted one of the AWS OpsWorks Stacks security groups and need to recreate it\.

**Cause:** These security groups are usually deleted by accident\.

**Solution:** The recreated group must an exact duplicate of the original, including the same capitalization for the group name\. Instead of recreating the group manually, the preferred approach is to have AWS OpsWorks Stacks perform the task for you\. Just create a new stack in the same AWS region—and VPC, if present—and AWS OpsWorks Stacks will automatically recreate all built\-in security groups, including the one that you deleted\. You can then delete the stack if you don't have any further use for it; the security groups will remain\.

## Chef Log Terminates Abruptly<a name="common-issues-troubleshoot-log-terminates"></a>

**Problem:** A Chef log terminates abruptly; the end of the log does not indicate a successful run or display an exception and stack trace\.

**Cause:** This behavior is typically caused by inadequate memory\.

**Solution:** Create a larger instance and use the agent CLI `run_command` command to run the recipes again\. For more information, see [Executing Recipes](troubleshoot-debug-cli.md#troubleshoot-debug-cli-recipes)\.

## Cookbook Does Not Get Updated<a name="common-issues-troubleshoot-update"></a>

**Problem:** You updated your cookbooks but the stack's instances are still running the old recipes\.

**Cause:** AWS OpsWorks Stacks caches cookbooks on each instance, and runs recipes from the cache, not the repository\. When you start a new instance, AWS OpsWorks Stacks downloads your cookbooks from the repository to the instance's cache\. However, if you subsequently modify your custom cookbooks, AWS OpsWorks Stacks does not automatically update the online instances' caches\. 

**Solution:** Run the[Update Cookbooks stack command](workingstacks-commands.md) to explicitly direct AWS OpsWorks Stacks to update your online instances' cookbook caches\.

## Instances are Stuck at Booting Status<a name="common-issues-troubleshoot-booting"></a>

**Problem:** When you restart an instance, or auto healing restarts it automatically, the startup operation stalls at the `booting` status\.

**Cause:** One possible cause of this issue is the VPC configuration, including a default VPC\. Instances must always be able to communicate with the AWS OpsWorks Stacks service, Amazon S3, and the package, cookbook, and app repositories\. If, for example, you remove a default gateway from a default VPC, the instances lose their connection to the AWS OpsWorks Stacks service\. Because AWS OpsWorks Stacks can no longer communicate with the instance [agent](troubleshoot-debug-cli.md), it treats the instance as failed and [auto heals](workinginstances-autohealing.md) it\. However, without a connection, AWS OpsWorks Stacks cannot install an instance agent on the healed instance\. Without an agent, AWS OpsWorks Stacks cannot run the Setup recipes on the instance, so the startup operation cannot progress beyond the "booting" status\. 

**Solution:** Modify your VPC configuration so that instances have the required connectivity\.

## Instances Unexpectedly Restart<a name="common-issues-troubleshoot-restart"></a>

**Problem:** A stopped instance unexpectedly restarts\. 

**Cause 1:** If you have enabled [auto healing](workinginstances-autohealing.md) for your instances, AWS OpsWorks Stacks periodically performs a health check on the associated Amazon EC2 instances, and restarts any that are unhealthy\. If you stop or terminate an AWS OpsWorks Stacks\-managed instance by using the Amazon EC2 console, API, or CLI, AWS OpsWorks Stacks is not notified\. Instead, it will perceive the stopped instance as unhealthy and automatically restart it\.

**Solution:** Manage your instances only by using the AWS OpsWorks Stacks console, API, or CLI\. If you use AWS OpsWorks Stacks to stop or delete an instance, it will not be restarted\. For more information, see [Manually Starting, Stopping, and Rebooting 24/7 Instances](workinginstances-starting.md) and [Deleting AWS OpsWorks Stacks Instances](workinginstances-delete.md)\.

**Cause 2:** Instances can fail for a variety of reasons\. If you have auto healing enabled, AWS OpsWorks Stacks automatically restarts failed instances\.

**Solution:** This is normal operation; there is no need to do anything unless you do not want AWS OpsWorks Stacks to restart failed instances\. In that case, you should disable auto healing\.

## `opsworks-agent` Processes are Running on Instances<a name="common-issues-troubleshoot-agent"></a>

**Problem:** Several `opsworks-agent` processes are running on your instances\. For example:

```
aws 24543 0.0 1.3 172360 53332 ? S Feb24 0:29 opsworks-agent: master 24543
aws 24545 0.1 2.0 208932 79224 ? S Feb24 22:02 opsworks-agent: keep_alive of master 24543
aws 24557 0.0 2.0 209012 79412 ? S Feb24 8:04 opsworks-agent: statistics of master 24543
aws 24559 0.0 2.2 216604 86992 ? S Feb24 4:14 opsworks-agent: process_command of master 24
```

**Cause:** These are legitimate processes that are required for the agent's normal operation\. They perform tasks such as handling deployments and sending keep\-alive messages back to the service\.

**Solution:** This is normal behavior\. Do not stop these processes; doing so will compromise the agent's operation\.

## Unexpected execute\_recipes Commands<a name="common-issues-troubleshoot-unexpected"></a>

**Problem:** The **Logs** section on an instance's details page includes unexpected `execute_recipes` commands\. Unexpected `execute_recipes` commands can also appear on the **Stack** and **Deployments** pages\.

**Cause:** This issue is often caused by permission changes\. When you change a user or group's SSH or sudo permissions, AWS OpsWorks Stacks runs `execute_recipes` to update the stack's instances and also triggers a Configure event\. Another source of `execute_recipes` commands is AWS OpsWorks Stacks updating the instance agent\.

**Solution:** This is normal operation; there is no need to do anything\.

To see what actions an `execute_recipes` command performed, go to the **Deployments** page and click the command's time stamp\. This opens the command's details page, which lists the key recipes that were run\. For example, the following details page is for an `execute_recipes` command that ran `ssh_users` to update SSH permissions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/command_details.png)

To see all of the details, click **show** in the command's **Log** column to display the associated Chef log\. Search the log for **Run List**\. AWS OpsWorks Stacks maintenance recipes will be under `OpsWorks Custom Run List`\. For example, the following is the run list for the `execute_recipes` command shown in the preceding screenshot, and shows every recipe that is associated with the command\.

```
[2014-02-21T17:16:30+00:00] INFO: OpsWorks Custom Run List: ["opsworks_stack_state_sync",
  "ssh_users", "test_suite", "opsworks_cleanup"]
```