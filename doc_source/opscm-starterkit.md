# Configure the Chef Server Using the Starter Kit<a name="opscm-starterkit"></a>

While Chef server creation is still in progress, open its Properties page in the AWS OpsWorks for Chef Automate console\. The first time that you work with a new Chef server, the Properties page prompts you to download two required items\. Download these items before your Chef server is online; the download buttons are not available after a new server is online\.

![\[AWS OpsWorks for Chef Automate new server properties page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_serverpropsdownload.png)
+ **Sign\-in credentials for the Chef server\.** You will use these credentials to sign in to the Chef Automate dashboard, where you work with Chef Automate premium features, such as workflow and compliance scans\. AWS OpsWorks Stacks does not save these credentials; this is the last time that they are available for viewing and downloading\. If necessary, you can change the password that is provided with these credentials after you sign in\.
+ **Starter Kit\.** The Starter Kit contains a README file with examples, a `knife.rb` configuration file, and a private key for the primary, or pivotal, user\. A new key pair is generated—and the old key is reset—each time you download the Starter Kit\.

In addition to the credentials that work only with the new server, the Starter Kit \.zip file includes a simple example of a Chef repository that works with any AWS OpsWorks for Chef Automate server\. In the Chef repository, you store cookbooks, roles, configuration files, and other artifacts for managing your nodes with Chef\. We recommend that you store this repository in a version control system, such as Git, and treat it as source code\. For information and examples that show how to set up a Chef repository that is tracked in Git, see [About the chef\-repo](https://docs.chef.io/chef_repo.html) in the Chef documentation\.

## Prerequisites<a name="finish-server-prereqs"></a>

1. While server creation is still in progress, download the sign\-in credentials for the Chef server, and save them in a secure but convenient location\.

1. Download the Starter Kit, and unzip the Starter Kit \.zip file into your workspace directory\. Do not share the Starter Kit private key\. If other users will be managing the Chef server, add them as administrators in the Chef Automate dashboard later\. For more information about how to add users to the Chef server, see [Manage Users](https://docs.chef.io/delivery_users_and_roles.html#manage-users) in the Chef Automate documentation\.

1. Download and install the Chef Development Kit, or [Chef DK](https://downloads.chef.io/chef-dk), on the computer you will use to manage your Chef server and nodes\. The [https://docs.chef.io/knife.html](https://docs.chef.io/knife.html) utility is part of the Chef DK\. For instructions, see [Install the Chef DK](https://docs.chef.io/release/devkit/install_dk.html) on the Chef website\.

## Explore the Starter Kit Contents<a name="w4ab1b9c23c11c13"></a>

The Starter Kit has the following contents\.
+ `site-cookbooks/` \- A directory for cookbooks that you create\. The `site-cookbooks/` folder contains the `opsworks-webserver` cookbook, a wrapper cookbook that depends on the `nginx` cookbook from the [Chef Supermarket](https://supermarket.chef.io/cookbooks/nginx) website\.
+ `Policyfile.rb` \- A Ruby\-based policy file that defines the cookbooks, dependencies, and attributes that become the policy for your nodes\.
+ `userdata.sh` and `userdata.ps1` \- You can use user data files to associate nodes automatically after launching your Chef Automate server\. `userdata.sh` is for bootstrapping Linux\-based nodes, and `userdata.ps1` is for Windows\-based nodes\.
+ `Berksfile` \- You can use this file if you prefer to use Berkshelf and `berks` commands to upload cookbooks and their dependencies\. In this walkthrough, we use `Policyfile.rb` and Chef commands to upload cookbooks, dependencies, and attributes\.
+ `README.md`, a Markdown\-based file that describes how to use the Starter Kit to set up your Chef Automate server for the first time\.
+ `.chef` is a hidden directory that contains a knife configuration file \(`knife.rb`\) and a secret authentication key file \(\.pem\)\.
  + `.chef/knife.rb` \- A knife configuration file \(`knife.rb`\)\. The [https://docs.chef.io/config_rb_knife.html](https://docs.chef.io/config_rb_knife.html) file is configured so that Chef's [https://docs.chef.io/knife.html](https://docs.chef.io/knife.html) tool operations run against the AWS OpsWorks for Chef Automate server\.
  + `.chef/ca_certs/opsworks-cm-ca-2016-root.pem` \- A certification authority \(CA\)\-signed SSL private key that is provided by AWS OpsWorks\. This key allows the server to identify itself to the Chef Infra client agent on nodes that your server manages\.

## Set Up Your Chef Repository<a name="w4ab1b9c23c11c15"></a>

A Chef repository contains several directories\. Each directory in the Starter Kit contains a README file that describes the directory's purpose, and how to use it for managing your systems with Chef\. There are two ways to get cookbooks installed on your Chef server: running `knife` commands, or running a Chef command to upload a policy file \(`Policyfile.rb`\) to your server that downloads and installs specified cookbooks\. This walkthrough uses Chef commands and `Policyfile.rb` to install cookbooks on your server\.

1. Create a directory on your local computer for storing cookbooks, such as `chef-repo`\. After you add cookbooks, roles, and other files to this repository, we recommend that you upload or store it in a secure, versioned system, such as CodeCommit, Git, or Amazon S3\.

1. In the `chef-repo` directory, create the following directories:
   + `site-cookbooks/`
   + `roles/` \- Stores roles in `.rb` or `.json` formats\.
   + `environments/` \- Stores environments in `.rb` or `.json` formats\.

## Use Policyfile\.rb to Get Cookbooks from a Remote Source<a name="install-cookbooks-policyfile"></a>

In this section, edit `Policyfile.rb` to specify cookbooks, then run a Chef command to upload the file to your server and install cookbooks\.

1. View `Policyfile.rb` in your Starter Kit\. By default, `Policyfile.rb` includes the `opsworks-webserver` wrapper cookbook , which depends on the [https://supermarket.chef.io/cookbooks/nginx](https://supermarket.chef.io/cookbooks/nginx) cookbook available on the Chef Supermarket website\. The `nginx` cookbook installs and configures a web server on managed nodes\. The required `chef-client` cookbook, which installs the Chef Infra client agent on managed nodes, is also specified\.

   `Policyfile.rb` also points to the optional Chef Audit cookbook, which you can use to set up compliance scans on nodes\. For more information about setting up compliance scans and getting compliance results for managed nodes, see [Compliance Scans in AWS OpsWorks for Chef Automate](opscm-chefcompliance.md)\. If you do not want to configure compliance scans and auditing right now, delete `'audit'` from the `run_list` section, and do not specify the `audit` cookbook attributes at the end of the file\.

   ```
   # Policyfile.rb - Describe how you want Chef to build your system.
   #
   # For more information on the Policyfile feature, visit
   # https://docs.chef.io/policyfile.html
   
   # A name that describes what the system you're building with Chef does.
   name 'opsworks-demo-webserver'
   
   # Where to find external cookbooks:
   default_source :supermarket
   
   # run_list: chef-client will run these recipes in the order specified.
   run_list  'chef-client',
             'opsworks-webserver',
             'audit'
   # add 'ssh-hardening' to your runlist to fix compliance issues detected by the ssh-baseline profile
   
   # Specify a custom source for a single cookbook:
   cookbook 'opsworks-webserver', path: 'site-cookbooks/opsworks-webserver'
   
   # Policyfile defined attributes
   
   # Define audit cookbook attributes
   default["opsworks-demo"]["audit"]["reporter"] = "chef-server-automate"
   default["opsworks-demo"]["audit"]["profiles"] = [
     {
       "name": "DevSec SSH Baseline",
       "compliance": "admin/ssh-baseline"
     }
   ]
   ```

   The following is an example of `Policyfile.rb` without the `audit` cookbook and attributes, if you want to configure only the `nginx` web server for now\.

   ```
   # Policyfile.rb - Describe how you want Chef to build your system.
   #
   # For more information on the Policyfile feature, visit
   # https://docs.chef.io/policyfile.html
   
   # A name that describes what the system you're building with Chef does.
   name 'opsworks-demo-webserver'
   
   # Where to find external cookbooks:
   default_source :supermarket
   
   # run_list: chef-client will run these recipes in the order specified.
   run_list  'chef-client',
             'opsworks-webserver'
   
   # Specify a custom source for a single cookbook:
   cookbook 'opsworks-webserver', path: 'site-cookbooks/opsworks-webserver'
   ```

   If you make changes to `Policyfile.rb`, be sure to save the file\.

1. Download and install the cookbooks defined in `Policyfile.rb`\.

   ```
   chef install
   ```

   All cookbooks are versioned in the cookbook's `metadata.rb` file\. Each time you change a cookbook, you must raise the version of the cookbook that is in its `metadata.rb`\.

1. If you have chosen to configure compliance scans, and kept the `audit` cookbook information in the policy file, push the policy `opsworks-demo` to your server\.

   ```
   chef push opsworks-demo
   ```

1. If you completed step 3, verify the installation of your policy\. Run the following command\.

   ```
   chef show-policy
   ```

   The results should resemble the following:

   ```
   opsworks-demo-webserver 
   ======================= 
   * opsworks-demo:  ec0fe46314
   ```

1. You are now ready to add or bootstrap nodes to your Chef Automate server\. You can automate the association of nodes by following steps in [Adding Nodes Automatically in AWS OpsWorks for Chef Automate](opscm-unattend-assoc.md), or add nodes one at a time by following steps in [Add Nodes for the Chef Server to Manage](opscm-addnodes.md)\. 

## \(Alternate\) Use Berkshelf to Get Cookbooks from a Remote Source<a name="opscm-berkshelf"></a>

Berkshelf is a tool for managing cookbooks and their dependencies\. If you prefer to use Berkshelf instead of `Policyfile.rb` to install cookbooks into local storage, use the procedure in this section instead of the preceding section\. You can specify which cookbooks and versions to use with your Chef server and upload them\. The Starter Kit contains a file named `Berksfile` that you can use to list your cookbooks\.

1. To get started, add the `chef-client` cookbook to the included Berksfile\. The `chef-client` cookbook configures the Chef Infra client agent software on each node that you connect to your Chef Automate server\. To learn more about this cookbook, see [Chef Client Cookbook](https://supermarket.chef.io/cookbooks/chef-client) in the Chef Supermarket\.

1. Using a text editor, append another cookbook to your Berksfile that installs a web server application; for example, the `apache2` cookbook, which installs the Apache web server\. Your Berksfile should resemble the following\.

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

1. Verify the installation of the cookbook by showing a list of cookbooks that are currently available on the Chef Automate server\. You can do this by running the following `knife` command\.

   You are ready to add nodes to manage with the AWS OpsWorks for Chef Automate server\.

   ```
   knife cookbook list
   ```