# Installing and Configuring the AWS CLI<a name="registered-instances-register-registering-cli"></a>

Before you register your first instance, you must install and configure a current version of the AWS CLI on the computer that you will run `register` from\. For more information about installing and configuring the AWS CLI, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

**Note**  
Although [AWS Tools for Windows PowerShell](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-welcome.html) includes the [http://docs.aws.amazon.com/powershell/latest/reference/items/Register-OPSInstance.html](http://docs.aws.amazon.com/powershell/latest/reference/items/Register-OPSInstance.html) cmdlet, which calls the `register` API action, we recommend that you use the AWS CLI to run the `register` command instead\.

You must run `register` with appropriate permissions\. You typically handle this task by installing IAM user credentials with appropriate permissions on the workstation or instance to be registered\. You can then run register with those credentials, as described later\.

**Note**  
If you run `register` on an Amazon EC2 instance, you should ideally use an IAM role to provide credentials\. For more information about how to attach an IAM role to an existing instance, see [Attach an AWS IAM Role to an Existing Amazon EC2 Instance by Using the AWS CLI](https://aws.amazon.com/blogs/security/new-attach-an-aws-iam-role-to-an-existing-amazon-ec2-instance-by-using-the-aws-cli/) \(blog post\)\.

You specify permissions by attaching an IAM policy to the IAM user or role\. For `register`, you usually attach the AWSOpsWorksRegisterCLI policy, which grants permissions to register both on\-premises and Amazon EC2 instances\.

To examine the AWSOpsWorksRegisterCLI policy, see [The AWSOpsWorksRegisterCLI Policy](registered-instances-register-registering-template.md)\. For more information about creating and managing AWS credentials, see [AWS Security Credentials](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)\. 


+ [Using Installed Credentials](#registered-instances-register-registering-cli-creds)
+ [Using an IAM Role](#registered-instances-register-registering-cli-role)

## Using Installed Credentials<a name="registered-instances-register-registering-cli-creds"></a>

There are several ways to install IAM user credentials on a system and provide them to an AWS CLI command\. The following describes the preferred approach and assumes that you will create a new IAM user\. You can also use an existing IAM user's credentials as long as the attached policies grant the required permissions\. For more information, including a description of other ways to install credentials, see [Configuration and Credential Files](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files)\.

**To use installed credentials**

1.  [Create an IAM User](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files) and save the access key ID and secret access key in a secure location\.

1. [Attach the AWSOpsWorksRegisterCLI policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) to the user\. If you prefer, you can attach a policy that grants broader permissions, as long as it includes the AWSOpsWorksRegisterCLI permissions\.

1. Create a profile for the user in the system's `credentials` file\. The file is located at `~/.aws/credentials` \(Linux, Unix, and OS X\) or `C:\Users\User_Name\.aws\credentials` \(Windows systems\)\. The file contains one or more profiles in the following format, each of which contains an IAM user's access key ID and secret access key\. 

   ```
   [profile_name]
   aws_access_key_id = access_key_id
   aws_secret_access_key = secret_access_key
   ```

   Substitute the IAM credentials that you saved earlier for the *access\_key\_id* and *secret\_access\_key* values\. You can specify any name you prefer for a profile name, with two limitations: the name must be unique and the default profile must be named `default`\. You can also use an existing profile, as long as it has the required permissions\. 

1. Use the `register` command's `--profile` argument to specify the profile name\. `register` will then run with the permissions that are granted to the associated credentials\.

   You can also omit `--profile`\. In that case, `register` runs with default credentials\. Be aware that these are not necessarily the default profile's credentials , so you must ensure that the default credentials have the required permissions\. For more information on how the AWS CLI determines default credentials, see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

## Using an IAM Role<a name="registered-instances-register-registering-cli-role"></a>

If you are running the command from the Amazon EC2 instance that you intend to register, the preferred strategy for providing credentials to `register` is to use an IAM role\. This approach allows you to avoid installing your credentials on the instance\. For more information about how to attach an IAM role to an existing instance, see [Attach an AWS IAM Role to an Existing Amazon EC2 Instance by Using the AWS CLI](https://aws.amazon.com/blogs/security/new-attach-an-aws-iam-role-to-an-existing-amazon-ec2-instance-by-using-the-aws-cli/) \(blog post\)\.

If the instance is running and has a role, you can grant the required permissions by attaching the AWSOpsWorksRegisterCLI policy to the role\. The role provides a set of default credentials for the instance\. As long as you have not installed any credentials on the instance, `register` automatically assumes the role and runs with its permissions\.

**Important**  
We recommend that you do not install credentials on the instance\. In addition to creating a security risk, the instance's role is at the end of the default providers chain that the AWS CLI uses to locate the default credentials\. Installed credentials might take precedence over the role, and `register` could therefore not have the required permissions\. For more information, see [Configuration Settings and Precedence](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#config-settings-and-precedence)\.

If a running instance does not have a role, you must install credentials with the required permissions on the instance, as described in [Using Installed Credentials](#registered-instances-register-registering-cli-creds)\. 