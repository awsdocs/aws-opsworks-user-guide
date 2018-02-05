# AWS OpsWorks for Chef Automate<a name="welcome_opscm"></a>

AWS OpsWorks for Chef Automate lets you run a [Chef Automate](https://www.chef.io/automate/) server in AWS\. You can provision a Chef server within minutes, and let AWS OpsWorks Stacks handle its operations, backups, restorations, and software upgrades\. AWS OpsWorks for Chef Automate frees you to focus on core configuration management tasks, instead of managing a Chef server\.

A Chef Automate server manages the configuration of nodes in your environment by instructing [https://docs.chef.io/chef_client.html](https://docs.chef.io/chef_client.html) which Chef recipes to run on the nodes, stores information about nodes, and serves as a central repository for your Chef cookbooks\. AWS OpsWorks for Chef Automate provides Chef servers and premium features of Chef Automate: visibility, workflow, and compliance\.

An AWS OpsWorks for Chef Automate server runs on an Amazon Elastic Compute Cloud instance\. AWS OpsWorks for Chef Automate servers are configured to run the newest version of Amazon Linux \(2017\.09\), Chef Server 12\.x, and the most current version of Chef Automate Server, version 1\.6\.x\.

The minimum supported version of `chef-client` on nodes associated with an AWS OpsWorks for Chef Automate server is 12\.16\.42\. We recommend running `chef-client` 13\.6\.4\.

When new minor versions of Chef software become available, system maintenance is designed to update the minor version of Chef Automate and Chef Server on the server automatically, as soon as it passes AWS testing\. AWS performs extensive testing to verify that Chef upgrades are production\-ready and do not disrupt existing customer environments, so there can be lags between Chef software releases and their availability for application to existing OpsWorks for Chef Automate servers\. System maintenance also upgrades your server to the newest version of Amazon Linux\.

You can connect any on\-premises computer or EC2 instance that is running a supported operating system and has network access to an AWS OpsWorks for Chef Automate server\. For a list of supported operating systems for nodes that you want to manage, see the [Chef website](https://docs.chef.io/platforms.html)\. The [https://docs.chef.io/chef_client.html](https://docs.chef.io/chef_client.html) agent software is installed on nodes that you want to manage with a Chef server\.


+ [Region Support for AWS OpsWorks for Chef Automate](#opscm-region)
+ [Getting Started with AWS OpsWorks for Chef Automate](gettingstarted-opscm.md)
+ [Back Up and Restore an AWS OpsWorks for Chef Automate Server](opscm-backup-restore.md)
+ [System Maintenance in AWS OpsWorks for Chef Automate](opscm-maintenance.md)
+ [Chef Compliance in AWS OpsWorks for Chef Automate](opscm-chefcompliance.md)
+ [Adding Nodes Automatically in AWS OpsWorks for Chef Automate](opscm-unattend-assoc.md)
+ [Disassociate a Node from an AWS OpsWorks for Chef Automate Server](opscm-disassociate-node.md)
+ [Delete an AWS OpsWorks for Chef Automate Server](opscm-delete-server.md)
+ [Reset Chef Automate Dashboard Credentials](opscm-resetchefcreds.md)
+ [Logging AWS OpsWorks for Chef Automate API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Troubleshooting AWS OpsWorks for Chef Automate](troubleshoot-opscm.md)

## Region Support for AWS OpsWorks for Chef Automate<a name="opscm-region"></a>

The following regional endpoints support AWS OpsWorks for Chef Automate servers\. AWS OpsWorks for Chef Automate creates resources that are associated with your Chef servers, such as instance profiles, IAM users, and service roles, in the same regional endpoint as your Chef server\. Your Chef server must be in a VPC\. You can use a VPC that you create or already have, or use the default VPC\.

+ US East \(Ohio\) Region

+ US East \(N\. Virginia\) Region

+ US West \(N\. California\) Region

+ US West \(Oregon\) Region

+ Asia Pacific \(Tokyo\) Region

+ Asia Pacific \(Singapore\) Region

+ Asia Pacific \(Sydney\) Region

+ EU \(Frankfurt\) Region

+ EU \(Ireland\) Region