# AWS OpsWorks document history<a name="history"></a>

| Change | Description | Date | 
| --- |--- |--- |
| [Updates to AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise](#history) | A troubleshooting procedure is now available that describes what you can do if system maintenance fails for your AWS OpsWorks for Chef Automate or OpsWorks for Puppet Enterprise server\. For more information, see [System maintenance fails for Chef Automate server](https://docs.aws.amazon.com/opsworks/latest/userguide/troubleshoot-opscm.html#tshooterrors-chef-maintenance-fails) or [System maintenance fails for Puppet Enterprise server\.](https://docs.aws.amazon.com/opsworks/latest/userguide/troubleshoot-opspup.html#tshooterrors-puppet-maintenance-fails) in this guide\. | September 29, 2022 | 
| [Updates to AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise](#history) | A troubleshooting procedure is now available if your AWS OpsWorks for Chef Automate or OpsWorks for Puppet Enterprise server enters a `Connection lost` state\. For more information, see [Chef Automate server is in a `Connection lost` state](https://docs.aws.amazon.com/opsworks/latest/userguide/troubleshoot-opscm.html#tshooterrors-chef-connection-lost) or [Puppet Enterprise server is in a `Connection lost` state](https://docs.aws.amazon.com/opsworks/latest/userguide/troubleshoot-opspup.html#tshooterrors-puppet-connection-lost) in this guide\. | March 23, 2022 | 
| [Updates to AWS OpsWorks Stacks](#history) | As a security best practice, you can now add an `aws:SourceArn` or `aws:SourceAccount` condition key \(or both\) to trust relationship policies that allow AWS OpsWorks Stacks access to perform tasks in other AWS services\. For more information, see [Cross\-service confused deputy prevention in AWS OpsWorks Stacks](https://docs.aws.amazon.com/opsworks/latest/userguide/cross-service-confused-deputy-prevention-stacks.html) in this guide\. | March 4, 2022 | 
| [Updates to AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise](#history) | As a security best practice, you can now add an `aws:SourceArn` or `aws:SourceAccount` condition key \(or both\) to trust relationship policies that allow AWS OpsWorks for Chef Automate and OpsWorks for Puppet Enterprise access to perform tasks in other AWS services\. For more information, see [Cross\-service confused deputy prevention](https://docs.aws.amazon.com/opsworks/latest/userguide/cross-service-confused-deputy-prevention.html) in this guide\. | January 10, 2022 | 
| [Updates to AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise](#history) | AWS OpsWorks for Chef Automate and OpsWorks for Puppet Enterprise have updated the managed policies [https://docs.aws.amazon.com/opsworks/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-awsopsworkscmservicerole](https://docs.aws.amazon.com/opsworks/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-awsopsworkscmservicerole) and [https://docs.aws.amazon.com/opsworks/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-awsopsworkscminstanceprofilerole](https://docs.aws.amazon.com/opsworks/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-awsopsworkscminstanceprofilerole), and now [store secrets in AWS Secrets Manager](https://docs.aws.amazon.com/opsworks/latest/userguide/data-protection.html#data-protection-secrets-manager)\. | May 3, 2021 | 
| [Updates to AWS OpsWorks for Puppet Enterprise](#history) | The engine version of an OpsWorks for Puppet Enterprise server that you create in the console is now 2019\.8\.5\. By using the API, you can specify either version `2019` or `2017` when you create a Puppet Enterprise server\. The `DescribeServers` API now returns an attribute called `PUPPET_API_CRL` in its results\. This attribute contains a certificate revocation list for internal use\. | April 28, 2021 | 
| [AWS OpsWorks Stacks uses a new managed policy](#history) | AWS OpsWorks Stacks has changed the managed policy that includes permissions to perform all actions in AWS OpsWorks Stacks\. The new policy is **AWSOpsWorks\_FullAccess**\. For more information about the permissions in this policy, see [Example policies](https://docs.aws.amazon.com/opsworks/latest/userguide/opsworks-security-users-examples.html)\. | February 19, 2021 | 
| [Migrate AWS OpsWorks Stacks stacks from EC2\-Classic to a VPC](#history) | Documentation has been added describing how to migrate an AWS OpsWorks Stacks stack from EC2\-Classic to a VPC\. | September 29, 2020 | 
| [Regenerate a starter kit for AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise](#history) | Documentation has been added describing how to regenerate the starter kit for an AWS OpsWorks for Chef Automate or an AWS OpsWorks for Puppet Enterprise server\. | July 29, 2020 | 
| [AWS OpsWorks for Puppet Enterprise lets you create a server that uses a custom domain, certificate, and private key](#history) | You can now create an OpsWorks for Puppet Enterprise server that uses a custom domain, certificate, and private key\. You can update an existing Puppet Enterprise server to use a custom domain by creating a server from a backup of an existing server\. | April 17, 2020 | 
| [AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise now support tagging in the console](#history) | You can now add tags to an AWS OpsWorks for Chef Automate server or an AWS OpsWorks for Puppet Enterprise master, or to server backups, by using either the AWS Management Console or the AWS CLI\. For more information, see [Work with Tags \(Chef\)](https://docs.aws.amazon.com/opsworks/latest/userguide/opscm-tags.html) or [Work with Tags \(Puppet\)](https://docs.aws.amazon.com/opsworks/latest/userguide/opspup-tags.html)\. | February 26, 2020 | 
| [AWS OpsWorks for Chef Automate simplifies upgrade of existing Chef Automate 1 servers to Chef Automate 2](#history) | You can upgrade eligible AWS OpsWorks for Chef Automate servers running Chef Automate 1 to Chef Automate 2 by choosing **Start upgrade** on your server's details page in the console, or by running the `StartMaintenance` API action\. For more information, see [Upgrade an AWS OpsWorks for Chef Automate Server to Chef Automate 2](https://docs.aws.amazon.com/opsworks/latest/userguide/opscm-a2upgrade.html)\. | January 24, 2020 | 
| [AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise](#history) | A new chapter about Security in AWS OpsWorks CM \(AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise\) has been added to the guide\. | December 23, 2019 | 
| [AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise support tagging](#history) | You can now add tags to an AWS OpsWorks for Chef Automate server or an AWS OpsWorks for Puppet Enterprise master, or to server backups, by using the AWS CLI\. AWS OpsWorks CM now supports tag\-based authorization\. | December 18, 2019 | 
| [AWS OpsWorks for Chef Automate lets you create a server that uses a custom domain, certificate, and private key](#history) | You can now create an AWS OpsWorks for Chef Automate 2\.0 server that uses a custom domain, certificate, and private key\. You can update an existing Chef Automate 2\.0 server to use a custom domain by creating a server from a backup of an existing server\. | October 22, 2019 | 
| [AWS OpsWorks Stacks now supports Ruby 2\.6\.1](#history) | AWS OpsWorks Stacks supports Ruby 2\.6\.1 on Rails App Server layers in Chef 11\.10 stacks\. | May 2, 2019 | 
| [AWS OpsWorks for Chef Automate now supports Chef Automate 2\.0](#history) | New AWS OpsWorks for Chef Automate servers will run Chef Automate 2\.0, which includes updates to Chef InSpec, new features in compliance scanning and reporting, and Chef Infra\. | April 30, 2019 | 
| [AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise](#history) | You can now use AWS CloudFormation to create an AWS OpsWorks for Chef Automate server or an AWS OpsWorks for Puppet Enterprise master server\. | January 24, 2019 | 
| [AWS OpsWorks Stacks](#history) | AWS OpsWorks Stacks now supports instances running Ubuntu 18\.04 LTS in Chef 12 stacks\. | December 18, 2018 | 
| [AWS OpsWorks for Puppet Enterprise](#history) | Added procedure for setting up an SSH\-based connection to a control repository that uses CodeCommit\. | December 3, 2018 | 
| [AWS OpsWorks Stacks](#history) | AWS OpsWorks Stacks now supports instances running Amazon Linux 2 in Chef 12 stacks\. | November 15, 2018 | 
| [AWS OpsWorks Stacks](#history) | AWS OpsWorks Stacks now supports instances running Amazon Linux 2018\.03 in Chef 11\.10 stacks\. | October 23, 2018 | 
| [AWS OpsWorks Stacks](#history) | AWS OpsWorks Stacks now supports instances running Amazon Linux 2018\.03 in Chef 12 stacks\. | August 23, 2018 | 
| [AWS OpsWorks for Chef Automate and OpsWorks for Puppet Enterprise](#history) | OpsWorks for Puppet Enterprise has upgraded to PE 2018\.1\.2\. AWS OpsWorks for Chef Automate has upgraded to Chef Automate 1\.8\.68\. | June 29, 2018 | 
+ **AWS OpsWorks for Chef Automate and OpsWorks for Puppet Enterprise API version:** 2016\-11\-01
+ **AWS OpsWorks Stacks API version: **2016\-03\-08
+ **Latest documentation update: **2022\-10\-18

## Earlier updates<a name="history-older"></a>

The following table describes important changes in each release of the *AWS OpsWorks User Guide* before June 2018\.


| Description | Date | 
| --- | --- | 
| AWS OpsWorks Stacks Chef version for Windows\-based stacks upgraded to 12\.22; Ruby version is now 2\.3\.6\. | April 19, 2018 | 
| New procedures for creating an AWS OpsWorks for Chef Automate server or an OpsWorks for Puppet Enterprise master by using the AWS CLI\. | March 23, 2018 | 
| Chef Automate version updated to 1\.8; Chef Compliance setup simplified with the addition of the opsworks\-audit cookbook\. | March 5, 2018 | 
| Added support for AWS OpsWorks Stacks events in Amazon CloudWatch Events\. | February 20, 2018 | 
| Added support for new EBS volume types in AWS OpsWorks Stacks, and a new API, DescribeOperatingSystems\. | January 25, 2018 | 
| OpsWorks for Puppet Enterprise and AWS OpsWorks for Chef Automate now support selecting multiple security groups when you are creating a server\. | January 18, 2018 | 
| Added support for AWS OpsWorks Stacks in the Europe \(Paris\) Region\. | December 19, 2017 | 
| Added support for AWS OpsWorks for Chef Automate and OpsWorks for Puppet Enterprise in six additional Regions, and added procedures for creating backups of AWS OpsWorks for Chef Automate and OpsWorks for Puppet Enterprise servers in the AWS Management Console\. | December 18, 2017 | 
| Added the new OpsWorks for Puppet Enterprise service and documentation\. | November 16, 2017 | 
| Added support for Amazon Linux 2017\.09 to AWS OpsWorks Stacks\. | November 7, 2017 | 
| Added support for Chef Compliance to AWS OpsWorks for Chef Automate\. | October 25, 2017 | 
| Added support for Amazon Linux 2017\.09 to AWS OpsWorks for Chef Automate\. | October 9, 2017 | 
| Added System Maintenance topic to AWS OpsWorks for Chef Automate chapter\. | July 28, 2017 | 
| Added support for tags in AWS OpsWorks Stacks\. | June 6, 2017 | 
| Added integration with CloudWatch Logs\. | April 10, 2017 | 
| Added the new AWS OpsWorks for Chef Automate service and documentation\. | December 1, 2016 | 
| Added support for the US East \(Ohio\) Region regional endpoint\. | October 12, 2016 | 
| Added support for stacks and instances that run the Amazon Linux 2016\.09 operating system\. | September 30, 2016 | 
| Added support for the Asia Pacific \(Seoul\) Region and nine additional regional endpoints\. | August 15, 2016 | 
| Added support for Node\.js 0\.12\.15 and Ruby 2\.3 in built\-in layers\. | July 6, 2016 | 
| Added support for the Asia Pacific \(Mumbai\) Region\. | June 28, 2016 | 
| Added support for stacks and instances that run the CentOS 7 operating system\. | June 22, 2016 | 
| Added walkthrough describing CodePipeline and AWS OpsWorks Stacks integration\. | June 2, 2016 | 
| Added support for stacks and instances that run the Ubuntu 16\.04 LTS operating system\. | June 1, 2016 | 
| Added Chef 12 Linux support and related documentation\.  | December 3, 2015 | 
| Added Node\.js walkthrough to Getting Started\. | July 14, 2015 | 
| Added two new cookbook examples to Cookbooks 101\. | July 14, 2015 | 
| Added support for agent version management\. | June 23, 2015 | 
| Added support for managing the agent version\. | June 24, 2015 | 
| Added support for custom Windows AMIs\. | June 22, 2015 | 
| Added three new Best Practices topics\. | June 11, 2015 | 
| Added support for Windows stacks\. | May 18, 2015 | 
| Added a Best Practices chapter\. | Dec\. 15, 2014 | 
| Added support for Elastic Load Balancing connection draining and custom Shutdown timeouts\. | Dec\. 15, 2014 | 
| Added support for registering instances created outside of AWS OpsWorks Stacks\. | Dec 9, 2014 | 
| Added support for Amazon SWF\. | Sept\. 4, 2014 | 
| Added support for associating environment variables with apps and extended Cookbooks 101\. | July 16, 2014 | 
| Added Cookbooks 101, a tutorial introduction to implementing cookbooks\. | July 16, 2014 | 
| Added support for CloudTrail\. | June 4, 2014 | 
| Added support for Amazon RDS\. | May 14, 2014 | 
| Added support for Chef 11\.10 and Berkshelf\. | Mar\. 27, 2014 | 
| Added support for Amazon EBS PIOPS volumes\. | Dec\. 16, 2013 | 
| Added resource\-based permissions\. | Dec\. 5, 2013 | 
| Added resource management\. | Oct\. 7, 2013 | 
| Added support for VPCs\. | Aug\. 29, 2013 | 
| Added support for custom AMIs and Chef 11\.4\. | July 24, 2013 | 
| Added console support for multiple layers per instance\. | July 1, 2013 | 
| Added support for Amazon EBS\-backed instances, Elastic Load Balancing, and Amazon CloudWatch monitoring\. | May 14, 2013 | 
| Initial release of the AWS OpsWorks Stacks User Guide\. | February 18, 2013 | 