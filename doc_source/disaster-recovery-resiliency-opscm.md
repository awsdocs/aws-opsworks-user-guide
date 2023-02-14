# Resilience in AWS OpsWorks CM<a name="disaster-recovery-resiliency-opscm"></a>

AWS OpsWorks CM enables daily backups of servers by default when you create a server\. Backups are encrypted and are stored in an Amazon S3 bucket\. By default, this bucket is accessible only to the account that created the server\. You can add bucket access for other users or configure cross\-region backups in Amazon S3 at your discretion\. Chef and Puppet support cross\-region encryption, because both products encrypt traffic between your AWS OpsWorks CM server and managed nodes\.

AWS OpsWorks CM does not support high availability \(HA\) configurations\.

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between Availability Zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

For more information about how to back up and restore servers in AWS OpsWorks CM, see the following:
+ [Back Up and Restore an OpsWorks for Puppet Enterprise Server](opspup-backup-restore.md)
+ [Back Up and Restore an AWS OpsWorks for Chef Automate Server](opscm-backup-restore.md)

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.