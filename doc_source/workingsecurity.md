# Security and Permissions<a name="workingsecurity"></a>

Each of your users must have appropriate AWS credentials to access your account's AWS resources\. The recommended way to provide credentials to users is with [AWS Identity and Access Management](http://aws.amazon.com/iam/) \(IAM\)\. AWS OpsWorks Stacks integrates with IAM to let you control the following:
+ How individual users can interact with AWS OpsWorks Stacks\.

  For example, you can allow some users to deploy apps to any stack but not modify the stack itself, while allowing other users full access but only to certain stacks, and so on\.
+ How AWS OpsWorks Stacks can act on your behalf to access stack resources such as Amazon EC2 instances and Amazon S3 buckets\.

  AWS OpsWorks Stacks provides a service role that grants permissions for these tasks\. 
+ How apps that run on Amazon EC2 instances controlled by AWS OpsWorks Stacks can access other AWS resources, such as data stored on Amazon S3 buckets\.

  You can assign an instance profile to a layer's instances that grants permissions to apps running on those instances to access other AWS resources\.
+ How to manage user\-based SSH keys and use SSH or RDP to connect to instances\.

  For each stack, administrative users can assign each IAM user a personal SSH key, or authorize users to specify their own key\. You can also authorize SSH or RDP access and sudo or administrator privileges on the stack's instances for each user\. 

Other aspects of security include the following:
+ How to manage updating your instances' operating system with the latest security patches\. 

  For more information, see [Managing Security Updates](workingsecurity-updates.md)\.
+ How to configure [Amazon EC2 security groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) to control network traffic to and from your instances\.

  How to specify custom security groups instead of the AWS OpsWorks Stacks default security groups\. For more information, see [Using Security Groups](workingsecurity-groups.md)\.

**Topics**
+ [Managing AWS OpsWorks Stacks User Permissions](opsworks-security-users.md)
+ [Signing in as an IAM User](workingsecurity-login.md)
+ [Allowing AWS OpsWorks Stacks to Act on Your Behalf](opsworks-security-servicerole.md)
+ [Specifying Permissions for Apps Running on EC2 instances](opsworks-security-appsrole.md)
+ [Managing SSH Access](security-ssh-access.md)
+ [Managing Linux Security Updates](workingsecurity-updates.md)
+ [Using Security Groups](workingsecurity-groups.md)