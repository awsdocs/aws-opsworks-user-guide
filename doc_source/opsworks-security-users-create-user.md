# Creating IAM Users for AWS OpsWorks Stacks<a name="opsworks-security-users-create-user"></a>

Before you can import IAM users into AWS OpsWorks Stacks, you need to create them\. You can do this using the [IAM console](https://console.aws.amazon.com/iam/), command line, or API\. For full instructions, see [Adding an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html)\.

Note that unlike [administrative users](opsworks-security-users-manage-admin.md), you don't need to attach a policy to define permissions\. You can set permissions after [importing the users into AWS OpsWorks Stacks](opsworks-security-users-manage-import.md), as explained in [Managing User Permissions](opsworks-security-users.md)\. 

For more information on creating IAM users and groups, see [IAM Getting Started](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAMGettingStarted.html)\.