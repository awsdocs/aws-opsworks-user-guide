# Infrastructure Security in AWS OpsWorks CM<a name="infrastructure-security-opscm"></a>

As a managed service, AWS OpsWorks CM is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

Use AWS published API calls to access AWS OpsWorks CM through the network\. AWS OpsWorks CM uses TLS 1\.2\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

AWS OpsWorks CM does not support private link or VPC private endpoints\.

AWS OpsWorks CM does not support resource\-based policies\. For more information, see [AWS Services that Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html#management_svcs) in the *AWS Identity and Access Management User Guide*\.