# Logging in to a Failed Instance<a name="troubleshoot-debug-login"></a>

If a recipe fails, the instance will end up in the `setup_failed` state instead of online\. Even though the instance is not online as far as AWS OpsWorks Stacks is concerned, the EC2 instance is running and it's often useful to log in to troubleshoot the issue\. For example, you can check whether an application or custom cookbook is correctly installed\. The AWS OpsWorks Stacks built\-in support for [SSH](workinginstances-ssh.md) and [RDP](workinginstances-rdp.md) login is available only for instances in the online state\. However, if you have assigned an SSH key pair to the instance, you can still log in, as follows:
+ Linux instances – Use the SSH key pair's private key to log in with a third\-party SSH client, such as OpenSSH or PuTTY\.

  You can use an EC2 key pair or your [personal SSH key pair](security-ssh-access.md) for this purpose\.
+ Windows instances – Use the EC2 key pair's private key to retrieve the instance's Administrator password\.

  Use that password to log in with your preferred RDP Client\. For more information, see [Logging in As Administrator](workinginstances-rdp.md#workinginstances-rdp-admin)\. You cannot use a [personal SSH key pair](security-ssh-access.md) to retrieve an Administrator password\.