# Installing and Configuring the AWS CLI<a name="registered-instances-register-registering-cli"></a>

Before you register your first instance, you must be running version 1\.16\.180 of the AWS CLI or newer on the computer from which you run `register`\. The installation details depend on your workstation's operating system\. For more information about installing the AWS CLI, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\. To check the version of the AWS CLI that you are running, enter `aws --version` in a shell session\.

**Note**  
Although [AWS Tools for PowerShell](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-welcome.html) includes the [http://docs.aws.amazon.com/powershell/latest/reference/items/Register-OPSInstance.html](http://docs.aws.amazon.com/powershell/latest/reference/items/Register-OPSInstance.html) cmdlet, which calls the `register` API action, we recommend that you use the AWS CLI to run the `register` command instead\.

You must run `register` with appropriate permissions\. You can get permissions by using an IAM role, or less optimally, by installing user credentials with appropriate permissions on the workstation or instance to be registered\. You can then run `register` with those credentials, as described later\. Specify permissions by attaching an IAM policy to the IAM user or role\. For `register`, you attach either the `AWSOpsWorksRegisterCLI_EC2` or `AWSOpsWorksRegisterCLI_OnPremises` policies, which grant permissions to register Amazon EC2 or on\-premises instances, respectively\.

**Note**  
If you run `register` on an Amazon EC2 instance, you should ideally use an IAM role to provide credentials\. For more information about how to attach an IAM role to an existing instance, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) or [Replacing an IAM Role](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#replace-iam-role) in the *Amazon EC2 User Guide*\.

For example snippets of the `AWSOpsWorksRegisterCLI_EC2` and `AWSOpsWorksRegisterCLI_OnPremises` policies, see [Instance Registration Policies](registered-instances-register-registering-template.md)\. For more information about creating and managing AWS credentials, see [AWS Security Credentials](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)\.

**Topics**
+ [Using an IAM Role](#registered-instances-register-registering-cli-role)
+ [Using Installed Credentials](#registered-instances-register-registering-cli-creds)

## Using an IAM Role<a name="registered-instances-register-registering-cli-role"></a>

If you are running the command from the Amazon EC2 instance that you intend to register, the preferred strategy for providing credentials to `register` is to use an IAM role that has the `AWSOpsWorksRegisterCLI_EC2` policy or equivalent permissions attached\. This approach allows you to avoid installing your credentials on the instance\. One way to do this is by using the **Attach/Replace IAM Role** command in the EC2 console, as shown in the following image\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/instance_register_attachrole.png)

For more information about how to attach an IAM role to an existing instance, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) or [Replacing an IAM Role](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#replace-iam-role) in the *Amazon EC2 User Guide*\. For instances that were launched with an instance profile \(recommended\), add the `--use-instance-profile` switch to your `register` command to provide credentials; do not use the `--profile` parameter\.

If the instance is running and has a role, you can grant the required permissions by attaching the `AWSOpsWorksRegisterCLI_EC2` policy to the role\. The role provides a set of default credentials for the instance\. As long as you have not installed any credentials on the instance, `register` automatically assumes the role and runs with its permissions\.

**Important**  
We recommend that you do not install credentials on the instance\. In addition to creating a security risk, the instance's role is at the end of the default providers chain that the AWS CLI uses to locate the default credentials\. Installed credentials might take precedence over the role, and `register` might therefore not have the required permissions\. For more information, see [Configuration Settings and Precedence](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#config-settings-and-precedence)\.

If a running instance does not have a role, you must install credentials with the required permissions on the instance, as described in [Using Installed Credentials](#registered-instances-register-registering-cli-creds)\. It is recommended, easier, and less error\-prone to use instances that are launched with an instance profile\.

## Using Installed Credentials<a name="registered-instances-register-registering-cli-creds"></a>

There are several ways to install IAM user credentials on a system and provide them to an AWS CLI command\. The following describes an approach that is no longer recommended, but can be used if you are registering EC2 instances that were launched without an instance profile\. You can also use an existing IAM user's credentials as long as the attached policies grant the required permissions\. For more information, including a description of other ways to install credentials, see [Configuration and Credential Files](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files)\.

**To use installed credentials**

1. [Create an IAM User](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files) and save the access key ID and secret access key in a secure location\.

1. [Attach the AWSOpsWorksRegisterCLI\_OnPremises policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) to the user\. If you prefer, you can attach a policy that grants broader permissions, as long as it includes the `AWSOpsWorksRegisterCLI_OnPremises` permissions\.

1. Create a profile for the user in the system's `credentials` file\. The file is located at `~/.aws/credentials` \(Linux, Unix, and OS X\) or `C:\Users\User_Name\.aws\credentials` \(Windows systems\)\. The file contains one or more profiles in the following format, each of which contains an IAM user's access key ID and secret access key\. 

   ```
   [profile_name]
   aws_access_key_id = access_key_id
   aws_secret_access_key = secret_access_key
   ```

   Substitute the IAM credentials that you saved earlier for the *access\_key\_id* and *secret\_access\_key* values\. You can specify any name you prefer for a profile name, with two limitations: the name must be unique, and the default profile must be named `default`\. You can also use an existing profile, as long as it has the required permissions\.

1. Use the `register` command's `--profile` parameter to specify the profile name\. The `register` command runs with the permissions that are granted to the associated credentials\.

   You can also omit `--profile`\. In that case, `register` runs with default credentials\. Be aware that these are not necessarily the default profile's credentials , so you must ensure that the default credentials have the required permissions\. For more information about how the AWS CLI determines default credentials, see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.