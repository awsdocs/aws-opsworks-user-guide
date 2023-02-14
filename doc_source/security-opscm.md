# Security in AWS OpsWorks Configuration Management \(CM\)<a name="security-opscm"></a>

Cloud security at AWS is the highest priority\. As an AWS customer, you benefit from a data center and network architecture that is built to meet the requirements of the most security\-sensitive organizations\.

Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud:
+ **Security of the cloud** – AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud\. AWS also provides you with services that you can use securely\. Third\-party auditors regularly test and verify the effectiveness of our security as part of the [AWS compliance programs](http://aws.amazon.com/compliance/programs/)\. To learn about the compliance programs that apply to AWS OpsWorks CM, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\.
+ **Security in the cloud** – Your responsibility is determined by the AWS service that you use\. You are also responsible for other factors including the sensitivity of your data, your company’s requirements, and applicable laws and regulations\. 

This documentation helps you understand how to apply the shared responsibility model when using AWS OpsWorks CM\. The following topics show you how to configure AWS OpsWorks CM to meet your security and compliance objectives\. You also learn how to use other AWS services that help you to monitor and secure your AWS OpsWorks CM resources\. 

**Topics**
+ [Data Protection in AWS OpsWorks CM](data-protection.md)
+ [Data Encryption](#protection-encryption-opscm)
+ [Identity and Access Management for AWS OpsWorks CM](security-iam-opscm.md)
+ [Internetwork Traffic Privacy](#protection-privacy-opscm)
+ [Logging and Monitoring in AWS OpsWorks CM](#sec-opsworks-log-mon-opscm)
+ [Compliance Validation for AWS OpsWorks CM](opsworks-stacks-compliance-opscm.md)
+ [Resilience in AWS OpsWorks CM](disaster-recovery-resiliency-opscm.md)
+ [Infrastructure Security in AWS OpsWorks CM](infrastructure-security-opscm.md)
+ [Configuration and Vulnerability Analysis in AWS OpsWorks CM](#sec-config-vulnerability-opscm)
+ [Security Best Practices for AWS OpsWorks CM](#sec-security-bestpractice-opscm)

## Data Encryption<a name="protection-encryption-opscm"></a>

AWS OpsWorks CM encrypts server backups and communication between authorized AWS users and their AWS OpsWorks CM servers\. However, the root Amazon EBS volumes of AWS OpsWorks CM servers are not encrypted\.

### Encryption at Rest<a name="protection-encryption-rest-opscm"></a>

AWS OpsWorks CM server backups are encrypted\. However, the root Amazon EBS volumes of AWS OpsWorks CM servers are not encrypted\. This is not user\-configurable\.

### Encryption in Transit<a name="protection-encryption-transit-opscm"></a>

AWS OpsWorks CM uses HTTP with TLS encryption\. AWS OpsWorks CM defaults to self\-signed certificates to provision and manage servers, if no signed certificate is provided by users\. We recommend that you use a certificate signed by a certificate authority \(CA\)\.

### Key Management<a name="protection-key-management-opscm"></a>

AWS Key Management Service customer managed keys and AWS managed keys are not currently supported by AWS OpsWorks CM\.

## Internetwork Traffic Privacy<a name="protection-privacy-opscm"></a>

AWS OpsWorks CM uses the same transmission security protocols generally used by AWS: HTTPS, or HTTP with TLS encryption\.

## Logging and Monitoring in AWS OpsWorks CM<a name="sec-opsworks-log-mon-opscm"></a>

AWS OpsWorks CM logs all API actions to CloudTrail\. For more information, see the following topics:
+ [Logging OpsWorks for Puppet Enterprise API Calls with AWS CloudTrail](logging-opspup-using-cloudtrail.md)
+ [Logging AWS OpsWorks for Chef Automate API Calls with AWS CloudTrail](logging-opsca-using-cloudtrail.md)

## Configuration and Vulnerability Analysis in AWS OpsWorks CM<a name="sec-config-vulnerability-opscm"></a>

AWS OpsWorks CM performs periodic kernel and security updates to the operating system that is running on your AWS OpsWorks CM server\. Users can set a window of time for automatic updates to occur for up to two weeks from the current date\. AWS OpsWorks CM pushes automatic updates of Chef and Puppet Enterprise minor versions\. For more information about configuring updates for AWS OpsWorks for Chef Automate, see [ System Maintenance \(Chef\)](opscm-maintenance.md) in this guide\. For more information about configuring updates for OpsWorks for Puppet Enterprise, see [System Maintenance \(Puppet\)](opspup-maintenance.md) in this guide\.

## Security Best Practices for AWS OpsWorks CM<a name="sec-security-bestpractice-opscm"></a>

AWS OpsWorks CM, like all AWS services, offers security features to consider as you develop and implement your own security policies\. The following best practices are general guidelines and don’t represent a complete security solution\. Because these best practices might not be appropriate or sufficient for your environment, treat them as helpful considerations rather than prescriptions\.
+ **Secure your Starter Kit and downloaded login credentials\.** When you create a new AWS OpsWorks CM server or download a new Starter Kit and credentials from the AWS OpsWorks CM console, store these items in a secure location that requires at least one factor of authentication at minimum\. The credentials provide administrator\-level access to your server\.
+ **Secure your configuration code\.** Secure your Chef or Puppet configuration code \(cookbooks and modules\) using recommended protocols for your source repositories\. For example, you can [restrict permissions to repositories in AWS CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control.html#auth-and-access-control-iam-access-control-identity-based), or follow [guidelines on the GitHub website for securing GitHub repositories](https://help.github.com/en/github/managing-security-vulnerabilities/adding-a-security-policy-to-your-repository)\.
+ **Use CA\-signed certificates to connect to nodes\.** Although you can use self\-signed certificates when you are registering or bootstrapping nodes on your AWS OpsWorks CM server, as a best practice, use CA\-signed certificates\. We recommend that you use a certificate signed by a certificate authority \(CA\)\.
+ **Do not share Chef or Puppet management console sign\-in credentials** with other users\. An administrator should create separate users for each user of the Chef or Puppet console websites\.
  + [Manage Users in Chef Automate](https://automate.chef.io/docs/users/)
  + [Manage Users in Puppet Enterprise](https://puppet.com/docs/pe/2017.3/rbac_user_roles_intro.html)
+ **Configure automatic backups and system maintenance updates\.** Configuring automatic maintenance updates on your AWS OpsWorks CM server helps ensure that your server is running the most current security\-related operating system updates\. Configuring automatic backups helps ease disaster recovery and speed restoration time in the event of an incident or failure\. Limit access to the Amazon S3 bucket that stores your AWS OpsWorks CM server backups; do not grant access to **Everyone**\. Grant read or write access to other users individually as needed, or create a security group in IAM for those users, and assign access to the security group\.
  + [ System Maintenance \(Chef\)](opscm-maintenance.md)
  + [System Maintenance \(Puppet\)](opspup-maintenance.md)
  + [Back Up and Restore an AWS OpsWorks for Chef Automate Server](opscm-backup-restore.md)
  + [Back Up and Restore an OpsWorks for Puppet Enterprise Server](opspup-backup-restore.md)
  + [Creating Your First IAM Delegated User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-delegated-user.html) in the *AWS Identity and Access Management User Guide*
  + [Security Best Practices for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/security-best-practices.html) in the *Amazon Simple Storage Service Developer Guide*