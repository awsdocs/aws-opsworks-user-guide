# Configure the Chef Server Using the Starter Kit<a name="opscm-starterkit"></a>

While Chef server creation is still in progress, open its Properties page in the AWS OpsWorks for Chef Automate console\. The first time that you work with a new Chef server, the Properties page prompts you to download two required items\. Download these items before your Chef server is online; the download buttons are not available after a new server is online\.

![\[AWS OpsWorks for Chef Automate new server properties page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_serverpropsdownload.png)
+ **Sign\-in credentials for the Chef server\.** You will use these credentials to sign in to the Chef Automate dashboard, where you work with Chef Automate premium features, such as workflow and compliance\. AWS OpsWorks Stacks does not save these credentials; this is the last time that they are available for viewing and downloading\. If necessary, you can change the password that is provided with these credentials after you sign in\.
+ **Starter Kit\.** The Starter Kit contains a README file with examples, a `knife.rb` configuration file, and a private key for the primary, or pivotal, user\. A new key pair is generated—and the old key is reset—each time you download the Starter Kit\.

In addition to the credentials that work only with the new server, the Starter Kit \.zip file includes a simple example of a Chef repository that works with any AWS OpsWorks for Chef Automate server\. In the Chef repository, you store cookbooks, roles, configuration files, and other artifacts for managing your nodes with Chef\. We recommend that you store this repository in a version control system, such as Git, and treat it as source code\. For information and examples that show how to set up a Chef repository that is tracked in Git, see [About the chef\-repo](https://docs.chef.io/chef_repo.html) in the Chef documentation\.

## Prerequisites<a name="finish-server-prereqs"></a>

1. While server creation is still in progress, download the sign\-in credentials for the Chef server, and save them in a secure, but convenient, location\.

1. Download the Starter Kit, and unzip the Starter Kit \.zip file into your workspace directory\. Do not share the Starter Kit private key\. If other users will be managing the Chef server, add them as administrators in the Chef Automate dashboard later\. For more information about how to add users to the Chef server, see [Manage Users](https://docs.chef.io/delivery_users_and_roles.html#manage-users) in the Chef Automate documentation\.

1. Download and install the Chef Development Kit, or [Chef DK](https://downloads.chef.io/chef-dk), on the computer you will use to manage your Chef server and nodes\. The [https://docs.chef.io/knife.html](https://docs.chef.io/knife.html) utility is part of the Chef DK\. For instructions, see [Install the Chef DK](https://docs.chef.io/release/devkit/install_dk.html) on the Chef website\.

## Configure Your Server with the `knife` Utility<a name="w4ab1b9c21c11c13"></a>

The `.chef` directory is hidden, and contains the following files: 
+ `.chef/knife.rb` \- A knife configuration file \(knife\.rb\)\. The [https://docs.chef.io/config_rb_knife.html](https://docs.chef.io/config_rb_knife.html) file is configured so that Chef's [https://docs.chef.io/knife.html](https://docs.chef.io/knife.html) tool operations run against the AWS OpsWorks for Chef Automate server\. 
+ `.chef/ca_certs/opsworks-cm-ca-2016-root.pem` \- A certification authority \(CA\)\-signed SSL private key that is provided by AWS OpsWorks\. This key allows the server to identify itself to the chef\-client agent on nodes that your server manages\.

## Set Up Your Chef Repository<a name="w4ab1b9c21c11c15"></a>

A Chef repository contains several directories\. Each directory in the Starter Kit contains a README file that describes the directory's purpose, and how to use it for managing your systems with Chef\. There are two ways to get cookbooks installed on your Chef server: `knife` commands, or Berkshelf commands\. This walkthrough uses Berkshelf to install cookbooks on your server\.

1. Create a directory on your local computer for storing cookbooks, such as `chef-repo`\. After you add cookbooks, roles, and other files to this repository, we recommend that you upload or store it in a secure, versioned system, such as AWS CodeCommit, Git, or Amazon S3\.

1. In the `chef-repo` directory, create the following three directories, as shown in the Starter Kit:
   + `cookbooks/` \- Stores cookbooks that you download or create\.
   + `roles/` \- Stores roles in `.rb` or `.json` formats\.
   + `environments/` \- Stores environments in `.rb` or `.json` formats\.

## Use Berkshelf to Get Cookbooks from a Remote Source<a name="opscm-berkshelf"></a>

Berkshelf is a tool for managing cookbooks and their dependencies\. It downloads a specified cookbook into local storage, which is called the Berkshelf\. You can specify which cookbooks and versions to use with your Chef server and upload them\.

The Starter Kit contains a file, named `Berksfile`, that lists your cookbooks\. The included Berksfile references the chef\-client cookbook that configures the Chef client agent software on each node that you connect to your Chef server\. To learn more about this cookbook, see [Chef Client Cookbook](https://supermarket.chef.io/cookbooks/chef-client) in the Chef Supermarket\.

1. Using a text editor, append another cookbook to your Berksfile in which to install the web server software; for example, to install the Apache web server application\. Your Berksfile should resemble the following\.

   ```
   source 'https://supermarket.chef.io'
   cookbook 'chef-client'
   cookbook 'apache2'
   ```

1. Download and install the cookbooks on your local computer\.

   ```
   berks install
   ```

1. Upload the cookbook to the Chef server\.

   On Linux, run the following\.

   ```
   SSL_CERT_FILE='.chef/ca_certs/opsworks-cm-ca-2016-root.pem' berks upload
   ```

   On Windows, run the following Chef DK command in a PowerShell session\. Before you run the command, be sure to set the execution policy in PowerShell to `RemoteSigned`\. Add `chef shell-init` to make Chef DK utility commands available to PowerShell\.

   ```
   $env:SSL_CERT_FILE="ca_certs\opsworks-cm-ca-2016-root.pem"
   chef shell-init berks upload
   Remove-Item Env:\SSL_CERT_FILE
   ```

1. Verify the installation of the cookbook by showing a list of cookbooks that are currently available on the Chef Automate server\.

   You are ready to add nodes to manage with the AWS OpsWorks for Chef Automate server\.

   ```
   knife cookbook list
   ```