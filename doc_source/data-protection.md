# Data Protection in AWS OpsWorks CM<a name="data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in AWS OpsWorks Configuration Management\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with OpsWorks CM or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into OpsWorks CM or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

The names of OpsWorks CM servers are not encrypted\.

OpsWorks CM collects the following customer data in the course of creating and maintaining your AWS OpsWorks for Chef Automate and AWS OpsWorks for Puppet Enterprise servers\.
+ For OpsWorks for Puppet Enterprise, we collect private keys that Puppet Enterprise uses to enable communication between your Puppet master and managed nodes\.
+ For AWS OpsWorks for Chef Automate, we collect private keys for certificates that you attach to the service if you are using a custom domain\. The private key that you provide when you are creating a Chef Automate server with a custom domain is passed through to your server\.

OpsWorks CM servers store your configuration code, such as Chef cookbooks or Puppet Enterprise modules\. Though this code is stored in server backups, AWS does not have access to it\. This content is encrypted, and only administrators in your AWS account can access it\. We recommend that you secure your Chef or Puppet configuration code using recommended protocols for your source repositories\. For example, you can [restrict permissions to repositories in AWS CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control.html#auth-and-access-control-iam-access-control-identity-based), or follow [guidelines on the GitHub website for securing GitHub repositories](https://help.github.com/en/github/managing-security-vulnerabilities/adding-a-security-policy-to-your-repository)\.

OpsWorks CM does not use customer\-provided content to maintain the service, or keep customer logs\. Logs about your OpsWorks CM servers are stored in your account, in Amazon S3 buckets\. IP addresses of users who connect to your OpsWorks CM servers are logged by AWS\.

## Integration with AWS Secrets Manager<a name="data-protection-secrets-manager"></a>

Starting May 3, 2021, when you create a new server in OpsWorks CM, OpsWorks CM stores secrets for the server in AWS Secrets Manager\. For new servers, the following attributes are stored as secrets in Secrets Manager\.
+ **Chef Automate server**
  + HTTPS private key \(only servers that do not use a custom domain\)
  + Chef Automate administrative password \(CHEF\_AUTOMATE\_ADMIN\_PASSWORD\)
+ **Puppet Enterprise master**
  + HTTPS private key \(only servers that do not use a custom domain\)
  + Puppet administrative password \(PUPPET\_ADMIN\_PASSWORD\)
  + Puppet r10k remote \(PUPPET\_R10K\_REMOTE\)

For existing servers that do not use a custom domain, the only secret stored in Secrets Manager, for both Chef Automate and Puppet Enterprise servers, is the HTTPS private key, because this is generated during automatic, weekly system maintenance\.

OpsWorks CM stores secrets in Secrets Manager automatically, and this behavior is not user\-configurable\.