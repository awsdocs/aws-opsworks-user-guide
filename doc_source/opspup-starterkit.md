# Configure the Puppet Master Using the Starter Kit<a name="opspup-starterkit"></a>

While Puppet master creation is still in progress, the server's Properties page opens in the OpsWorks for Puppet Enterprise console\. The first time that you work with a new Puppet master, the Properties page prompts you to download two required items\. Download these items before your Puppet server is online; the download buttons are not available after a new server is online\.

![\[OpsWorks for Puppet Enterprise new server properties page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_creatingserver.png)
+ **Sign\-in credentials for the Puppet master\.** You will use these credentials to sign in to the Puppet Enterprise console, where you perform most node management\. AWS OpsWorks does not save these credentials; this is the last time that they are available for viewing and downloading\. If necessary, you can change the password that is provided with these credentials after you sign in\.
+ **Starter Kit\.** The Starter Kit contains a README file with information and examples describing how to finish setup, and administrator credentials for the Puppet Enterprise console\. New credentials are generated—and the old credentials invalidated—each time you download the Starter Kit\.

## Prerequisites<a name="finish-server-prereqs-puppet"></a>

1. While server creation is still in progress, download the sign\-in credentials for the Puppet master, and save them in a secure but convenient location\.

1. Download the Starter Kit, and unzip the Starter Kit \.zip file into your workspace directory\. Do not share your sign\-in credentials\. If other users will be managing the Puppet master, add them as administrators in the Puppet Enterprise console later\. For more information about how to add users to the Puppet master, see [Creating and managing users and user roles](https://docs.puppet.com/pe/latest/rbac_user_roles.html#add-a-user-to-a-user-role) in the Puppet Enterprise documentation\.

## Install the Puppet Master Certificate<a name="opspup-post-launch"></a>

To work with your Puppet master and add nodes to manage, you'll need to install its certificate\. Install it by running the following AWS CLI command\. You cannot perform this task in the AWS Management Console\.

```
aws --region region opsworks-cm describe-servers --server-name server_name --query "Servers[0].EngineAttributes[?Name=='PUPPET_API_CA_CERT'].Value" --output text > .config/ssl/cert/ca.pem
```

## Generate a Short\-term Token<a name="opspup-post-launch-token"></a>

To use the Puppet API, you must create a short\-term token for yourself\. This step is not required to use the Puppet Enterprise console\. Generate the token by running the following command\.

The default token lifetime is five minutes, but you can change this default\. For more information about how to change the default token lifetime, see [Change the token's default lifetime](https://puppet.com/docs/pe/2017.3/rbac/rbac_token_auth_intro.html#change-the-token-default-lifetime) in the Puppet Enterprise documentation\.

```
puppet-access login --config-file .config/puppetlabs/client-tools/puppet-access.conf --lifetime 8h
```

**Note**  
Because the default token lifetime is five minutes, the preceding example command adds the `--lifetime` parameter to extend the token lifetime for a longer period\. You can set the token lifetime for a period of up to 10 years \(`10y`\)\. For more information about how to change the default token lifetime, see [Change the token's default lifetime](https://puppet.com/docs/pe/2017.3/rbac/rbac_token_auth_intro.html#change-the-token-default-lifetime) in the Puppet Enterprise documentation\.

## Set Up the Starter Kit Nginx Example<a name="w4ab1b7c19c11c15"></a>

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

For more information about how you can apply the sample Nginx configuration to managed nodes that you create in Amazon EC2, see [Adding Nodes Automatically in OpsWorks for Puppet Enterprise](opspup-unattend-assoc.md)\.