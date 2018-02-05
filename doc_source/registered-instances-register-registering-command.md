# Using the register Command<a name="registered-instances-register-registering-command"></a>

**Note**  
This feature is supported only for Linux stacks\.

The following shows the general syntax for the `register` command\.

```
aws opsworks register \
  [--profile profile_name] \
  [--region region_name] \
  --infrastructure-class instance_type \
  --stack-id stack ID \
  [--local] | [--ssh-private-key key_file --ssh-username username] | [--override-ssh command_string] \
  [--override-hostname hostname] \
  [--debug] \
  [--override-public-ip public IP] \
  [--override-private-ip private IP] \
..[--use-instance-profile] \
  [ [IP address] | [hostname] | [instance ID]
```

The following arguments are common to all AWS CLI commands\.

**`--profile`**  
\(Optional\) The credential's profile name\. If you omit this argument, the command runs with your default credentials\. For more information on how the AWS CLI determines default credentials, see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

**`--region`**  
 \(Optional\) The AWS OpsWorks Stacks service endpoint's region\. Do not set `--region` to the stack's region\. AWS OpsWorks Stacks automatically determines the stack's region from the stack ID\.  
If your default region is already set, you can omit this argument\. For more information on how to specify a default region, see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

Use the following arguments for both Amazon EC2 and on\-premises instances\.

**`--infrastructure-class`**  
\(Required\) This parameter must be set to either `ec2` or `on-premises`, to indicate whether you are registering an Amazon EC2 or on\-premises instance, respectively\.

**`--stack-id`**  
\(Required\) The ID of the stack that the instance is to be registered with\.  
To find a stack ID, on the **Stack** page, click **Settings**\. The stack ID is labeled **OpsWorks ID**, and is a GUID that looks something like `ad21bce6-7623-47f1-bf9d-af2affad8907`\.

**SSH Login Arguments**  
Use the following arguments to specify how `register` should log in to the instance\.    
**`--local`**  
\(Optional\) Use this argument to register the instance that you run the command on\.   
In this case, `register` does not need to log in to the instance\.  
**`--ssh-private-key` and `--ssh-username`**  
 \(Optional\) Use these arguments if you are registering the instance from a separate workstation and want to explicitly specify the user name or private key file\.  

+ `--ssh-username` – Use this argument to specify an SSH user name\.

  If you omit `--ssh-username`, `ssh` uses the default user name\.

+ `--ssh-private-key` – Use this argument to explicitly specify a private key file\.

  If you omit `--ssh-private-key`, `ssh` will attempt to log in using authentication techniques that do not require a password, including using the default private key\. If none of those techniques are supported, `ssh` queries for your password\. For more information on how `ssh` handles authentication, see [The Secure Shell \(SSH\) Authentication Protocol](https://www.ietf.org/rfc/rfc4252.txt)\.  
**`--override-ssh`**  
 \(Optional\) Use this argument if you are registering the instance from a separate workstation and want to specify a custom [http://linux.about.com/od/commands/l/blcmdl1_ssh.htm](http://linux.about.com/od/commands/l/blcmdl1_ssh.htm) command string\. The `register` command will use this command string to log in to the registered instance\. 
For more information on `ssh`, see [SSH](http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/slogin.1)\.

**`--override-hostname`**  
 \(Optional\) Specifies a host name for the instance, which is used only by AWS OpsWorks Stacks\. The default value is the instance's host name\.

**\-\-debug**  
\(Optional\) Provides debugging information if the registration process fails\. For troubleshooting information, see [Troubleshooting Instance Registration](common-issues.md#common-issues-instance-registration)\.

**\-\-use\-instance\-profile**  
\(Optional\) Lets the `register` command use an attached instance profile, instead of creating an IAM user\. Adding this parameter can help prevent errors that occur if you try to manage a registered instance when the IAM user has accidentally been deleted\.

**Target**  
\(Conditional\) If you run this command from a workstation, the final value in the command string specifies the registration target in one of the following ways\.  

+ The instance's public IP address\.

+ The instance's host name\.

+ For Amazon EC2 instances, the instance ID\.

  AWS OpsWorks Stacks uses the instance ID to obtain the instance configuration, including the instance's public IP address\. By default, AWS OpsWorks Stacks uses this address to construct the `ssh` command string that it uses to log in to the instance\. If you need to connect to a private IP address, you must use `--override-ssh` to provide a custom command string\. For an example, see [Register an On\-Premises Instance from a Workstation](registered-instances-register-registering-examples.md#registered-instances-register-registering-examples-workstation-onprem)\.
If you specify a host name, `ssh` depends on the DNS server to resolve the name to a particular instance\. If you aren't certain that the host name is unique, use `ssh` to verify that the host name resolves to the correct instance\.
If you run this command from the instance to be registered, omit the instance identifier and instead use the `--local` argument\.

The following arguments are only for on\-premises instances\.

**`--override-public-ip`**  
\(Optional\) AWS OpsWorks Stacks displays the specified address as the instance's public IP address\. It does not change the instance's public IP address\. However, if a user uses the console to connect to the instance, such as by clicking the address on the **Instances** page, AWS OpsWorks Stacks uses the specified address\. AWS OpsWorks Stacks automatically determines the argument's default value\.

**`--override-private-ip`**  
\(Optional\) AWS OpsWorks Stacks displays the specified address as the instance's private IP address\. It does not change the instance's private IP address\. AWS OpsWorks Stacks automatically determines the argument's default value\. 