# Configure the Puppet Master Using the Starter Kit<a name="opspup-starterkit"></a>

While Puppet master creation is still in progress, the server's Properties page opens in the AWS OpsWorks for Puppet Enterprise console\. The first time that you work with a new Puppet master, the Properties page prompts you to download two required items\. Download these items before your Puppet server is online; the download buttons are not available after a new server is online\.

![\[AWS OpsWorks for Puppet Enterprise new server properties page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_creatingserver.png)

+ **Sign\-in credentials for the Puppet master\.** You will use these credentials to sign in to the Puppet Enterprise console, where you perform most node management\. AWS OpsWorks does not save these credentials; this is the last time that they are available for viewing and downloading\. If necessary, you can change the password that is provided with these credentials after you sign in\.

+ **Starter Kit\.** The Starter Kit contains a README file with information and examples describing how to finish setup, and administrator credentials for the Puppet Enterprise console\. New credentials are generated—and the old credentials invalidated—each time you download the Starter Kit\.

## Prerequisites<a name="finish-server-prereqs-puppet"></a>

1. While server creation is still in progress, download the sign\-in credentials for the Puppet master, and save them in a secure but convenient location\.

1. Download the Starter Kit, and unzip the Starter Kit \.zip file into your workspace directory\. Do not share your sign\-in credentials\. If other users will be managing the Puppet master, add them as administrators in the Puppet Enterprise console later\. For more information about how to add users to the Puppet master, see [Creating and managing users and user roles](https://docs.puppet.com/pe/latest/rbac_user_roles.html#add-a-user-to-a-user-role) in the Puppet Enterprise documentation\.

## Set Up the Starter Kit Nginx Example<a name="w3ab2b7c15c11c11"></a>

After you download and unzip the Starter Kit, you can use the example branch in the included, sample `control-repo-example` folder to configure an Nginx web server on your managed nodes\. 

The Starter Kit includes two `control-repo` folders: `control-repo`, and `control-repo-example`\. The `control-repo` folder includes a `production` branch that is unchanged from what you would see in the [Puppet GitHub repository](https://github.com/puppetlabs/control-repo)\. The `control-repo-example` folder also has a `production` branch that includes example code to set up a Nginx server with a test website\.

1. Push the `control-repo-example` `production` branch to your Git remote \(the `r10k_remote` URL of your Puppet master\)\. In your Starter Kit root directory, run the following, replacing *r10kRemoteUrl* with your `r10k_remote` URL\.

   ```
   cd  control-repo-example
   git remote add origin r10kRemoteUrl
   git push origin production
   ```

   Puppet's Code Manager uses Git branches as environments\. By default, all nodes are in the production environment\. 
**Important**  
Do not push to a `master` branch\. The `master` branch is reserved for the Puppet master\.

1. Deploy the code in the `control-repo-example` branch to your Puppet master\. This lets the Puppet Master download your Puppet code from your Git repository \(`r10k_remote`\)\. In your Starter Kit root directory, run the following\.

   ```
   puppet-code deploy --all --wait --config-file .config/puppet-code.conf
   ```

For more information about how you can apply the sample Nginx configuration to managed nodes that you create in Amazon EC2, see \.

## Set Up Authentication for Code Manager<a name="w3ab2b7c15c11c15"></a>

To securely deploy your environments, Code Manager requires an authentication token\. To generate a token for Code Manager, assign a user to the deployment role, and then request an authentication token\. You can complete this procedure by following steps in the section [Set up authentication for Code Manager](https://puppet.com/docs/pe/2017.3/code_management/code_mgr_config.html#set-up-authentication-for-code-manager) in the Puppet Enterprise documentation\.
