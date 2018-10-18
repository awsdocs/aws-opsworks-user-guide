# Adding Apps<a name="workingapps-creating"></a>

The first step in deploying an application to your application servers is to add an app to the stack\. The app represents the application, and contains a variety of metadata, such as the application's name and type, and the information required to deploy the application to the server instances, such as the repository URL\. You must have Manage permissions to add an app to a stack\. For more information, see [Managing User Permissions](opsworks-security-users.md)\.

**Note**  
The procedure in this section applies to Chef 12 and newer stacks\. For information about how to add apps to layers in Chef 11 stacks, see [Step 2\.4: Create and Deploy an App \- Chef 11](gettingstarted-simple-app.md)\.

**To add an app to a stack**

1. Put the code in your preferred repository—an Amazon S3 archive, a Git repository, a Subversion repository, or an HTTP archive\. For more information, see [Application Source](#workingapps-creating-source)\.

1. Click **Apps** in the navigation pane\. On the **Apps** page, click **Add an app** for your first app\. For subsequent apps, click **\+App**\. 

1. Use the **App New **page to configure the app, as described in the following section\.

## Configuring an App<a name="workingapps-creating-general"></a>

The **Add App** page consists of the following sections: **Settings**, **Application source**, **Data Sources**, **Environment Variables**, **Add Domains**, and **SSL Settings**\.

**Topics**
+ [Settings](#workingapps-creating-settings)
+ [Application Source](#workingapps-creating-source)
+ [Data Sources](#workingapps-creating-data)
+ [Environment Variables](#workingapps-creating-environment)
+ [Domain and SSL Settings](#workingapps-creating-domain-ssl)

### Settings<a name="workingapps-creating-settings"></a>

**Name**  
The app name, which is used to represent the app in the UI\. AWS OpsWorks Stacks also uses this name to generate a short name for the app that is used internally and to identify the app in the [stack configuration and deployment attributes](workingcookbook-json.md)\. After you have added the app to the stack, you can see the short name by clicking **Apps** in the navigation pane and then clicking the app's name to open the details page\.

****Document root****  
AWS OpsWorks Stacks assigns the **Document root** setting to the [`[:document_root]`](attributes-json-deploy.md#attributes-json-deploy-app-root) attribute in the app's `deploy` attributes\. The default value is `null`\. Your deployment recipes can obtain that value from the `deploy` attributes using standard Chef node syntax and deploy the specified code to the appropriate location on the server\. For more information about how to deploy apps, see [Deploy Recipes](create-custom-deploy.md)\.

### Application Source<a name="workingapps-creating-source"></a>

You can deploy apps from the following repository types: Git, Amazon S3 bundle, HTTP bundle, and Other\. All repository types require you to specify the repository type and the repository URL\. Individual repository types have their own requirements, as explained below\.

**Note**  
AWS OpsWorks Stacks automatically deploys applications from the standard repositories to the built\-in server layers\. If you use the Other repository type, which is the only option for Windows stacks, AWS OpsWorks Stacks puts the repository information in the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy), but you must implement custom recipes to handle the deployment tasks\.

**Topics**
+ [HTTP Archive](#w4ab1c11c49c11b9b8b8)
+ [Amazon S3 Archive](#w4ab1c11c49c11b9b8c10)
+ [Git Repository](#w4ab1c11c49c11b9b8c12)
+ [Other Repositories](#w4ab1c11c49c11b9b8c14)

#### HTTP Archive<a name="w4ab1c11c49c11b9b8b8"></a>

To use a publicly\-accessible HTTP server as a repository: 

1. Create a compressed archive—zip, gzip, bzip2, Java WAR, or tarball—of the folder that contains the app's code and any associated files\.
**Note**  
AWS OpsWorks Stacks does not support uncompressed tarballs\.

1. Upload the archive file to the server\. 

1. To specify the repository in the console, select HTTP Archive as the repository type and enter the URL\.

    If the archive is password\-protected, under **Application Source**, specify the user name and password\.

#### Amazon S3 Archive<a name="w4ab1c11c49c11b9b8c10"></a>

To use an Amazon Simple Storage Service bucket as a repository:

1. Create a public or private Amazon S3 bucket\. For more information, see [Amazon S3 Documentation](http://aws.amazon.com/documentation/s3/)\.

1. For AWS OpsWorks Stacks to access private buckets, you must be an IAM user with at least read\-only rights to the Amazon S3 bucket and you will need the access key ID and secret access key\. For more information, see [AWS Identity and Access Management \(IAM\) Documentation](http://aws.amazon.com/documentation/iam/)\.

1. Put the code and any associated files in a folder and store the folder in a compressed archive—zip, gzip, bzip2, Java WAR, or tarball\.
**Note**  
AWS OpsWorks Stacks does not support uncompressed tarballs\.

1. Upload the archive file to the Amazon S3 bucket and record the URL\.

1. To specify the repository in the AWS OpsWorks Stacks console, set **Repository type** to **S3 Archive** and enter the archive's URL\. For a private archive, you must also provide an AWS access key ID and secret access key whose policy grants permissions to access the bucket\. Leave these settings blank for public archives\.

#### Git Repository<a name="w4ab1c11c49c11b9b8c12"></a>

A [Git](http://git-scm.com/) repository provides source control and versioning\. AWS OpsWorks Stacks supports publicly hosted repository sites such as [GitHub](https://github.com/) or [Bitbucket](https://bitbucket.org) as well as privately hosted Git servers\. For both apps and Git submodules, the format you use to specify the repository's URL in **Application Source** depends on whether the repository is public or private:

**Public repository**–Use the HTTPS or Git read\-only protocols\. For example, [Getting Started with Chef 11 Linux Stacks](gettingstarted.md) uses a public GitHub repository that can be accessed by either of the following URL formats:
+ Git read\-only: **git://github\.com/amazonwebservices/opsworks\-demo\-php\-simple\-app\.git**
+ HTTPS: **https://github\.com/amazonwebservices/opsworks\-demo\-php\-simple\-app\.git**

**Private repository**–Use the SSH read/write format shown in these examples:
+ Github repositories: **git@github\.com:*project*/*repository***\.
+ Repositories on a Git server: ***user*@*server*:*project*/*repository***

Selecting **Git** under **Source Control** displays two additional optional settings:

**Repository SSH key**  
You must specify a deploy SSH key to access private Git repositories\. This field requires the private key; the public key is assigned to your Git repository\. For Git submodules, the specified key must have access to those submodules\. For more information, see [Using Git Repository SSH Keys](workingapps-deploykeys.md)\.  
The deploy SSH key cannot require a password; AWS OpsWorks Stacks has no way to pass it through\.

**Branch/Revision**  
If the repository has multiple branches, AWS OpsWorks Stacks downloads the master branch by default\. To specify a particular branch, enter the branch name, SHA1 hash, or tag name\. To specify a particular commit, enter the full 40\-hexdigit commit identifier\.

#### Other Repositories<a name="w4ab1c11c49c11b9b8c14"></a>

If the standard repositories do not meet your requirements, you can use other repositories, such as [Bazaar](http://bazaar.canonical.com/en/)\. However, AWS OpsWorks Stacks does not automatically deploy apps from such repositories\. You must implement custom recipes to handle the deployment process and assign them to the appropriate layers' Deploy events\. For an example of how to implement Deploy recipes, see [Deploy Recipes](create-custom-deploy.md)\.

### Data Sources<a name="workingapps-creating-data"></a>

This section attaches a database to the app\. You have the following options: 
+ **RDS** – Attach one of the stack's [Amazon RDS service layers](workinglayers-db-rds.md)\.
+ **None** – Do not attach a database server\.

If you select **RDS**, you must specify the following\.

**Database instance**  
The list includes every Amazon RDS service layer\. You can also select one of the following:  
\(Required\) Specify which database server to attach to the app\. The contents of the list depend on the data source\.  
+ **RDS** – A list of the stack's Amazon RDS service layers\. 

**Database name**  
\(Optional\) Specify a database name\.   
+ Amazon RDS layer – Enter the database name that you specified for the Amazon RDS instance\.

  You can get the database name from the [Amazon RDS console](https://console.aws.amazon.com/rds/)\.

When you deploy an app with an attached database, AWS OpsWorks Stacks adds the database instance's connection to the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy)\.

You can write a custom recipe to retrieve the information from the `deploy` attributes and put it file that can be accessed by the application\. This is the only option for providing database connection information to the Other application type\.

For more information on how to handle database connections, see [Connecting to a Database](workingapps-connectdb.md)\.

To detach a database server from an app, [edit the app's configuration](workingapps-editing.md) to specify a different database server, or no server\.

### Environment Variables<a name="workingapps-creating-environment"></a>

You can specify a set of environment variables for each app, which are specific to the app\. For example, if you have two apps, the environment variables that you define for the first app are not available to the second app and vice versa\. You can also define the same environment variable for multiple apps and assign it a different value for each app\. 

**Note**  
There is no specific limit on the number of environment variables\. However, the size of the associated data structure—which includes the variables' names, values, and protected flag values—cannot exceed 20 KB\. This limit should accommodate most if not all use cases\. Exceeding it will cause a service error \(console\) or exception \(API\) with the message, "Environment: is too large \(maximum is 20 KB\)\."

AWS OpsWorks Stacks stores the variables as attributes in the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy)\. You can have your custom recipes retrieve those values by using standard Chef node syntax\. For examples of how to access an app's environment variables, see [Using Environment Variables](apps-environment-vars.md)\.

**Key**  
The variable name\. It can contain up to 64 upper and lower case letters, numbers, and underscores \(\_\), but it must start with a letter or underscore\.

**Value**  
The variable's value\. It can contain up to 256 characters, which must all be printable\. 

**Protected value**  
Whether the value is protected\. This setting allows you to conceal sensitive information such as passwords\. If you set **Protected value** for a variable, after you create the app:  
+ The app's details page displays only the variable name, not the value\.
+ If you have permission to edit the app, you can click **Update value** to specify a new value, but you cannot see or edit the old value\.

**Note**  
Chef deployment logs can sometimes include environment variables\. This means protected variables might be shown in the console\. To prevent protected variables from being shown in the console, we recommend that you use Amazon S3 buckets as storage for protected variables that you do not want shown in the console\. An example of how to use an S3 bucket for this purpose is available in [Using an Amazon S3 Bucket](gettingstarted.walkthrough.photoapp.md) in this guide\.

### Domain and SSL Settings<a name="workingapps-creating-domain-ssl"></a>

For the Other app type, AWS OpsWorks Stacks adds the settings to the app's `deploy` attributes\. Your recipes can retrieve the data from those attributes and configure the server as needed\.

**Domain Settings**  
This section has an optional **Add Domains** field for specifying domains\. For more information, see [Using Custom Domains](workingapps-domains.md)\. 

**SSL Settings**  
This section has an **SSL Support** toggle that you can use to enable or disable SSL\. If you click **Yes**, you'll need to provide SSL certificate information\. For more information, see [Using SSL](workingsecurity-ssl.md)\.