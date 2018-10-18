# Installing Custom Cookbooks<a name="workingcookbook-installingcustom-enable"></a>

To have a stack install and use custom cookbooks, you must configure the stack to enable custom cookbooks, if it is not already configured\. You must then provide the repository URL and any related information such as a password\.

**Important**  
After you have configured the stack to support custom cookbooks, AWS OpsWorks Stacks automatically installs your cookbooks on all new instances at startup\. However, you must explicitly direct AWS OpsWorks Stacks to install new or updated cookbooks on any existing instances by running the [**Update Custom Cookbooks** stack command](workingstacks-commands.md)\. For more information, see [Updating Custom Cookbooks](workingcookbook-installingcustom-enable-update.md)\. Before you enable **Use custom Chef cookbooks** on your stack, be sure that custom and community cookbooks that you run support the version of Chef that your stack uses\.

**To configure a stack for custom cookbooks**

1. On your stack's page, click **Stack Settings** to display its **Settings** page\., Click **Edit** to edit the settings\.

1. Toggle **Use custom Chef cookbooks** to **Yes**\.  
![\[Editing the stack settings page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/stack_settings_edit.png)

1. Configure your custom cookbooks\.

When you are finished, click **Save** to save the updated stack\. 

## Specifying a Custom Cookbook Repository<a name="workingcookbook-installingcustom-enable-repo"></a>

Linux stacks can install custom cookbooks from any of the following repository types:
+ HTTP or Amazon S3 archives\.

  They can be either public or private, but Amazon S3 is typically the preferred option for a private archive\. 
+ Git and Subversion repositories provide source control and the ability to have multiple versions\.

Windows stacks can install custom cookbooks from Amazon S3 archives and Git repositories\.

All repository types have the following required fields\.
+ **Repository type**–The repository type
+ **Repository URL**–The repository URL

AWS OpsWorks Stacks supports publicly hosted Git repository sites such as [GitHub](https://github.com/) or [Bitbucket](https://bitbucket.org) as well as privately hosted Git servers\. For Git repositories, you must use one of the following URL formats, depending on whether the repository is public or private\. Follow the same URL guidelines for Git submodules\.

For a public Git repository, use the HTTPS or Git read\-only protocols:
+ Git read\-only – `git://github.com/amazonwebservices/opsworks-example-cookbooks.git`\.
+ HTTPS – `https://github.com/amazonwebservices/opsworks-example-cookbooks.git`\.

For a private Git repository, you must use the SSH read/write format, as shown in the following examples:
+ Github repositories – `git@github.com:project/repository`\.
+ Repositories on a Git server – `user@server:project/repository`

The remaining settings vary with the repository type and are described in the following sections\.

### HTTP Archive<a name="workingcookbook-installingcustom-enable-repo-http"></a>

Selecting **Http Archive** for **Repository type** displays two additional settings, which you must complete if the archive is password protected\.
+ **User name**–Your user name
+ **Password**–Your password

### Amazon S3 Archive<a name="workingcookbook-installingcustom-enable-repo-s3"></a>

Selecting **S3 Archive** for **Repository type** displays the following additional, optional settings\. AWS OpsWorks Stacks can access your repository by using Amazon EC2 roles \(host operating system manager authentication\), whether you use the AWS OpsWorks Stacks API or console\.
+ **Access key ID** –An AWS access key ID, such as AKIAIOSFODNN7EXAMPLE\.
+ **Secret access key** – The corresponding AWS secret access key, such as wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY\.

### Git Repository<a name="workingcookbook-installingcustom-enable-repo-git"></a>

Selecting **Git** under **Source Control** displays the following additional optional settings:

**Repository SSH key**  
You must specify a deploy SSH key to access private Git repositories\. For Git submodules, the specified key must have access to those submodules\. For more information, see [Using Git Repository SSH Keys](workingapps-deploykeys.md)\.  
The deploy SSH key cannot require a password; AWS OpsWorks Stacks has no way to pass it through\.

**Branch/Revision**  
If the repository has multiple branches, AWS OpsWorks Stacks downloads the master branch by default\. To specify a particular branch, enter the branch name, SHA1 hash, or tag name\. To specify a particular commit, enter the full 40\-hexdigit commit ID\.

### Subversion Repository<a name="workingcookbook-installingcustom-enable-repo-svn"></a>

Selecting **Subversion** under **Source Control** displays the following additional settings:
+ **User name**–Your user name, for private repositories\.
+ **Password**–Your password, for private repositories\.
+ **Revision**–\[Optional\] The revision name, if you have multiple revisions\.

  To specify a branch or tag, you must modify the repository URL, for example: **http://repository\_domain/repos/myapp/branches/my\-apps\-branch** or **http://repository\_domain\_name/repos/calc/myapp/my\-apps\-tag**\.