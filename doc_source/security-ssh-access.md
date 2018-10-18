# Managing SSH Access<a name="security-ssh-access"></a>

AWS OpsWorks Stacks supports SSH keys for both Linux and Windows stacks\.
+ For Linux instances, you can use SSH to log in to an instance, for example, to run [agent CLI](agent.md) commands\.

  For more information, see [Logging In with SSH](workinginstances-ssh.md)\.
+ For Windows instances, you can use an SSH key to obtain the instance's Administrator password, which you can then use to log in with RDP\.

  For more information, see [Logging In with RDP](workinginstances-rdp.md)\.

Authentication is based on an SSH key pair, which consists of a public key and a private key:
+ You install the public key on the instance\.

  The location depends on the particular operating system, but AWS OpsWorks Stacks handles the details for you\.
+ You store the private key locally and provide it to an SSH client, such as `ssh.exe`, to access the instance\.

  The SSH client uses the private key to connect to the instance\.

To provide SSH access to a stack's users, you need a way to create SSH key pairs, install public keys on the stack's instances, and securely manage the private keys\.

Amazon EC2 provides a simple way to install a public SSH key on an instance\. You can use the Amazon EC2 console or API to create one or more key pairs for each AWS region that you plan to use\. Amazon EC2 stores the public keys on AWS and you store the private keys locally\. When you launch an instance, you specify one of the region's key pairs and Amazon EC2 automatically installs it on the instance\. You then use the corresponding private key to log in to the instance\. For more information, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

With AWS OpsWorks Stacks, you can specify one of the region's Amazon EC2 key pairs when you create a stack, and optionally override it with a different key pair when you create each instance\. When AWS OpsWorks Stacks launches the corresponding Amazon EC2 instance, it specifies the key pair and Amazon EC2 installs the public key on the instance\. You can then use the private key to log in or retrieve an Administrator password, just as you would with a standard Amazon EC2 instance\. For more information, see [Installing an Amazon EC2 Key](security-settingec2key.md)\.

Using an Amazon EC2 key pair is convenient, but has two significant limitations:
+ An Amazon EC2 key pair is tied to a particular AWS region\.

  If you work in multiple regions, you must manage multiple key pairs\.
+ You can install only one Amazon EC2 key pair on an instance\.

  If you want to allow multiple users to log in, they must all have a copy of the private key, which is not a recommended security practice\.

For Linux stacks, AWS OpsWorks Stacks provides a simpler and more flexible way to manage SSH key pairs\.
+ Each user registers a personal key pair\.

  They store the private key locally and register the public key with AWS OpsWorks Stacks, as described in [Registering an IAM User's Public SSH Key](security-settingsshkey.md)\. 
+ When you set user permissions for a stack, you specify which users should have SSH access to the stack's instances\.

  AWS OpsWorks Stacks automatically creates a system user on the stack's instances for each authorized user and installs their public key\. The user can then use the corresponding private key to log in, as described in [Logging In with SSH](workinginstances-ssh.md)\.

Using personal SSH keys has the following advantages\.
+ There's no need to manually configure keys on the instances; AWS OpsWorks Stacks automatically installs the appropriate public keys on every instance\.
+ AWS OpsWorks Stacks installs only authorized users' personal public keys\.

  Unauthorized users cannot use their personal private key to gain access to instances\. With Amazon EC2 key pairs, any user with the corresponding private key can log in, with or without authorized SSH access\. 
+ If a user no longer needs SSH access, you can use the [**Permissions** page](opsworks-security-users-manage-edit.md) to revoke the user's SSH/RDP permissions\. 

  AWS OpsWorks Stacks immediately uninstalls the public key from the stack's instances\.
+ You can use the same key for any AWS region\.

  Users have to manage only one private key\.
+ There is no need to share private keys\.

  Each user has his or her own private key\.
+ It's easy to rotate keys\.

  You or the user updates the public key in **My Settings** and AWS OpsWorks Stacks automatically updates the instances\.