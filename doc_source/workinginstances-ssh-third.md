# Using a Third\-Party SSH Client<a name="workinginstances-ssh-third"></a>

You can also use a third\-party SSH client, such as PuTTY, to connect to Linux instances\. 

**To use a third party SSH client**

1. Ensure that AWS OpsWorks Stacks has installed an Amazon EC2 public key or an IAM user's personal public key on the instance, as discussed earlier\.

1. Obtain the instance's public DNS name or public IP address from its details page\.

1. Provide the client with the instance's host name, which depends on the operating system, as follows:
   + Amazon Linux and Red Hat Enterprise Linux \(RHEL\)– `ec2-user@DNSName/Address.`
   + Ubuntu – `ubuntu@DNSName/Address`\.

   Replace *DNSName/Address* with the public DNS name or IP address from the previous step\.

1. Provide the client with a private key that corresponds to an installed public key\. You can use either an Amazon EC2 private key or an IAM user's personal private key, depending on which public keys have been installed on the instance\.