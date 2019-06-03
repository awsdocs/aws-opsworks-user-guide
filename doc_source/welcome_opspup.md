# AWS OpsWorks for Puppet Enterprise<a name="welcome_opspup"></a>

OpsWorks for Puppet Enterprise lets you launch a [Puppet Enterprise](https://puppet.com/products/puppet-enterprise) master in minutes, and lets AWS OpsWorks handle its operations, backups, restorations, and software upgrades\. OpsWorks for Puppet Enterprise frees you to focus on core configuration management tasks, instead of managing a Puppet master\. By using OpsWorks for Puppet Enterprise, you can use the same configurations to manage both your on\-premises and cloud infrastructure, helping you to efficiently scale your operations in a hybrid environment\. Management of your Puppet master server is simplified by the Puppet Enterprise console, the AWS Management Console, and the AWS CLI\.

A Puppet master manages the configuration of nodes in your environment by serving configuration catalogs for specific nodes to the [https://docs.puppet.com/puppet/4.9/about_agent.html](https://docs.puppet.com/puppet/4.9/about_agent.html) software, and serves as a central repository for your Puppet modules\. A Puppet master in OpsWorks for Puppet Enterprise deploys `puppet-agent` to your managed nodes, and provides premium features of Puppet Enterprise\.

An OpsWorks for Puppet Enterprise master runs on an Amazon Elastic Compute Cloud instance\. OpsWorks for Puppet Enterprise servers are configured to run the newest version of Amazon Linux \(Amazon Linux 2\), and the most current version of Puppet Enterprise Master, version 2018\.1\.7\. For more information about changes in Puppet Enterprise 2018\.1\.7, see the [Puppet Enterprise Release Notes](https://puppet.com/docs/pe/2018.1/release_notes/release_notes.html)\.

When new versions of Puppet software become available, system maintenance is designed to update the version of Puppet Enterprise on the server automatically, as soon as it passes AWS testing\. AWS performs extensive testing to verify that Puppet upgrades are production\-ready and do not disrupt existing customer environments\.

You can connect any on\-premises computer or EC2 instance that is running a supported operating system and has network access to an OpsWorks for Puppet Enterprise master\. For a list of supported operating systems for nodes that you want to manage, see [Supported operating systems](https://docs.puppet.com/pe/latest/sys_req_os.html#puppet-agent-platforms) in the Puppet Enterprise documentation\. The [https://docs.puppet.com/puppet/4.9/about_agent.html](https://docs.puppet.com/puppet/4.9/about_agent.html) agent software is installed by the Puppet master on nodes that you want to manage\.

**Topics**
+ [Encryption in Puppet Enterprise](#opspup-encrypt)
+ [Region Support for OpsWorks for Puppet Enterprise](#opspup-region)
+ [Getting Started with OpsWorks for Puppet Enterprise](gettingstarted-opspup.md)
+ [Create an AWS OpsWorks for Puppet Enterprise Master by using AWS CloudFormation](opspup-create-server-cfn.md)
+ [Back Up and Restore an OpsWorks for Puppet Enterprise Server](opspup-backup-restore.md)
+ [System Maintenance in OpsWorks for Puppet Enterprise](opspup-maintenance.md)
+ [Adding Nodes Automatically in OpsWorks for Puppet Enterprise](opspup-unattend-assoc.md)
+ [Disassociate a Node from an OpsWorks for Puppet Enterprise Server](opspup-disassociate-node.md)
+ [Delete an OpsWorks for Puppet Enterprise Server](opspup-delete-server.md)
+ [Logging OpsWorks for Puppet Enterprise API Calls with AWS CloudTrail](logging-opspup-using-cloudtrail.md)
+ [Troubleshooting OpsWorks for Puppet Enterprise](troubleshoot-opspup.md)

## Encryption in Puppet Enterprise<a name="opspup-encrypt"></a>

For more information about encryption and security of your data and applications in Puppet, see [Securing an Application](https://puppet.com/docs/pipelines-for-apps/free/application-secure.html) on the Puppet website\.

## Region Support for OpsWorks for Puppet Enterprise<a name="opspup-region"></a>

The following regional endpoints support OpsWorks for Puppet Enterprise masters\. OpsWorks for Puppet Enterprise creates resources that are associated with your Puppet masters, such as instance profiles, IAM users, and service roles, in the same regional endpoint as your Puppet master\. Your Puppet master must be in a VPC\. You can use a VPC that you create or already have, or use the default VPC\.
+ US East \(Ohio\) Region
+ US East \(N\. Virginia\) Region
+ US West \(N\. California\) Region
+ US West \(Oregon\) Region
+ Asia Pacific \(Tokyo\) Region
+ Asia Pacific \(Singapore\) Region
+ Asia Pacific \(Sydney\) Region
+ EU \(Frankfurt\) Region
+ EU \(Ireland\) Region