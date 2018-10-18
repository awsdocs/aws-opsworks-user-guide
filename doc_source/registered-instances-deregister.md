# Deregistering a Registered Instance<a name="registered-instances-deregister"></a>

When you no longer want a registered instance in your stack, you can deregister it\. On the **Instances** page, choose the instance name and then choose **Deregister**\. AWS OpsWorks Stacks then does the following:
+ Unassigns the instance from any assigned layers\.
+ Shuts down and uninstalls the agent\.
+ Deregisters any attached resources \(Elastic IP addresses and Amazon EBS volumes\)\.

  This procedure includes resources that were attached to the instance prior to registration, and resources that you used AWS OpsWorks Stacks to attach to the instance while it was part of the stack\. After deregistration, the resources are no longer part of the stack's resources, but they remain attached to the instance\. 
+ Removes the instance from the stack\.
+ For on\-premises instances, stops the charges\.

The instance remains in the running state, but it is under your direct control and is no longer managed by AWS OpsWorks Stacks\. 

**Note**  
Both registering and deregistering computers or instances are supported only within Linux stacks\. Deregistration does not remove all changed files, and does not fully revert to backed\-up copies of certain files\. This list applies to both Chef 11\.10 and Chef 12 stacks; differences between the two versions are noted here\.  
`/etc/hosts` is backed up to `/var/lib/aws/opsworks/local-mode-cache/backup/etc/`, but is not restored\.
Entries remain for `aws` and `opsworks` in passwd, group, and shadow files, etc\.
`/etc/sudoers` contains a reference to an AWS OpsWorks Stacks directory\.
The following files are safe to leave behind; long\-term, consider deleting `/var/lib/aws/opsworks`\.  
`/var/log/aws/opsworks` remains on instances in Chef 11\.10 stacks\.
`/var/lib/aws/opsworks` remains on both Chef 11\.10 and Chef 12 stacks\.
`/var/chef` remains on instances in Chef 12 stacks\.
Other files left behind:  
`/etc/logrotate.d/opsworks-agent`
`/etc/cron.d/opsworks-agent-updater`
`/etc/ld.so.conf.d/opsworks-user-space.conf`
`/etc/motd.opsworks-static`
`/etc/aws/opsworks`
`/etc/sudoers.d/opsworks`
`/etc/sudoers.d/opsworks-agent`