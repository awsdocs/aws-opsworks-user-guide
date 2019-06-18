# Optional: Use CodeCommit as a Puppet r10k Remote Control Repository<a name="opspup-puppet-codecommit"></a>

You can create a new repository by using CodeCommit, and use it as your r10k remote control repository\. To complete steps in this section, and work with an CodeCommit repository, you need an IAM user that has the **AWSCodeCommitReadOnly** managed policy attached\. For more information about how to create an IAM user, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\.

**Topics**
+ [Step 1: Use CodeCommit as a Repository with an HTTPS connection type](#codecommit-puppet-https)
+ [Step 2: \(Optional\) Use CodeCommit as a Repository with an SSH connection type](#codecommit-puppet-ssh)

## Step 1: Use CodeCommit as a Repository with an HTTPS connection type<a name="codecommit-puppet-https"></a>

1. In the CodeCommit console, create a new repository\.  
![\[Creating new repository in CodeCommit.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_cc_remote.png)

1. Choose **Skip** to skip setting up an Amazon SNS topic\.

1. On the **Code** page, choose **Connect to your repository**\.

1. On the **Connect to your repository** page, choose **HTTPS** as the **Connection type**, and choose your operating system\.  
![\[Creating a new repository in CodeCommit.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_cc_connect.png)

   In the **Steps to clone your repository** area, your `git clone` URL should resemble the following: `https://git-codecommit.region.amazonaws.com/v1/repos/control-repo`\. Copy this URL to a convenient place for use in Puppet server setup\.

1. Close the **Connect to your repository** page, and return to the OpsWorks for Puppet Enterprise server setup\.

1. Paste the URL that you copied in Step 4 in the **r10k remote** string box in the **Configure credentials** page of the Puppet master setup wizard\. Leave the **r10k private key** box empty\. Finish creating and launching your Puppet master\.

1. In the IAM console, attach the **AWSCodeCommitReadOnly** policy to the instance profile role of your Puppet master\. For more information about how to attach a policy to an IAM role, see [Attaching Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console) in the *IAM User Guide*\.

1. Follow the steps in [Setup for HTTPS Users Using Git Credentials](http://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-gc.html) in the *CodeCommit User Guide* to push your existing `control-repo` content to the new CodeCommit repository\.

1. Now, you can continue by following the instructions in [Configure the Puppet Master Using the Starter Kit](opspup-starterkit.md), and use the Starter Kit to deploy code to your Puppet master\. The following command is an example\.

   ```
   puppet-code deploy --all --wait --config-file .config/puppet-code.conf
   ```

## Step 2: \(Optional\) Use CodeCommit as a Repository with an SSH connection type<a name="codecommit-puppet-ssh"></a>

You can configure an CodeCommit r10k remote control repository to use SSH key pair authentication\. The following prerequisites must be completed before you start this procedure\. 
+ You must have launched your OpsWorks for Puppet Enterprise server with an HTTPS control repository as described in the preceding section, [Step 1: Use CodeCommit as a Repository with an HTTPS connection type](#codecommit-puppet-https)\. This must be completed first so you can upload the required configuration to the Puppet master\.
+ Be sure you have an IAM user with the **AWSCodeCommitReadOnly** managed policy attached\. For more information about how to create an IAM user, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the *AWS Identity and Access Management User Guide*\.
+ Create and associate an SSH key with your IAM user\. Follow instructions for creating a public/private key pair with `ssh-keygen` in [Step 3: Configure Credentials on Linux, macOS, or Unix](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html#setting-up-ssh-unixes-keys) in the *CodeCommit User Guide*\.

1. In an AWS CLI session, run the following command to upload the private key file contents to AWS Systems Manager Parameter Store\. Your OpsWorks for Puppet Enterprise server queries this parameter to get a required certificate file\. Replace *private\_key\_file* with the path to your SSH private key file\.

   ```
   aws ssm put-parameter --name puppet_user_pk --type String --value "`cat private_key_file`"
   ```

1. Add Systems Manager Parameter Store permissions to your Puppet master\.

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the left navigation pane, choose **Roles**\.

   1. Choose **aws\-opsworks\-cm\-ec2\-role**\.

   1. On the **Permissions** tab, choose **Attach policies**\.

   1. In the **Search** bar, enter **AmazonSSMManagedInstanceCore**\.

   1. In the search results, choose **AmazonSSMManagedInstanceCore**\.

   1. Choose **Attach policy**\.

1. Create the configuration file manifest\. If you are using the `control-repo-example` repository provided in the starter kit, create the following files in the locations shown in the example repository\. Otherwise, create them according to your own control repository structure\. Replace the *IAM\_USER\_SSH\_KEY* value with the SSH key ID you created in the prerequisites for this procedure\.

   ```
   control-repo-example/site/profile/manifests/codecommit.pp
   ```

   ```
   class profile::codecommit {
     $configfile = @(CONFIGFILE)
         Host git-codecommit.*.amazonaws.com
         User IAM_USER_SSH_KEY
         IdentityFile /etc/puppetlabs/puppetserver/ssh/codecommit.rsa
         StrictHostKeyChecking=no
         | CONFIGFILE
   
     # Replace REGION with the correct region for your server.
     $command = @(COMMAND)
         aws ssm get-parameters \
         --region REGION \
         --names puppet_user_pk \
         --query "Parameters[0].Value" \
         --output text >| /etc/puppetlabs/puppetserver/ssh/codecommit.rsa
         | COMMAND
   
     $dirs = [
               '/opt/puppetlabs/server/data/puppetserver/.ssh',
               '/etc/puppetlabs/puppetserver/ssh',
             ]
   
     file { $dirs:
       ensure => 'directory',
       group  => 'pe-puppet',
       owner  => 'pe-puppet',
       mode   => '0750',
     }
   
     file { 'ssh-config':
       path    => '/opt/puppetlabs/server/data/puppetserver/.ssh/config',
       require => File[$dirs],
       content => $configfile,
       group   => 'pe-puppet',
       owner   => 'pe-puppet',
       mode    => '0600',
     }
   
     exec { 'download-codecommit-certificate':
       command => $command,
       require => File[$dirs],
       creates => '/etc/puppetlabs/puppetserver/ssh/codecommit.rsa',
       path    => '/bin',
       cwd     => '/etc/puppetlabs',
     }
   
     file { 'private-key-permissions':
       subscribe => Exec['download-codecommit-certificate'],
       path      => '/etc/puppetlabs/puppetserver/ssh/codecommit.rsa',
       group     => 'pe-puppet',
       owner     => 'pe-puppet',
       mode      => '0600',
     }
   }
   ```

1. Push your control repository to CodeCommit\. Run the following commands to push the new manifest file to your repository\.

   ```
   git add ./site/profile/manifests/codecommit.pp
   git commit -m 'Configuring for SSH connection to CodeCommit'
   git push origin production
   ```

1. Deploy the manifest files\. Run the following commands to deploy the updated configuration to your OpsWorks for Puppet Enterprise server\. Replace *STARTER\_KIT\_DIRECTORY* with the path to your Puppet configuration files\.

   ```
   cd STARTER_KIT_DIRECTORY
   
   puppet-access login --config-file .config/puppetlabs/client-tools/puppet-access.conf
   
   puppet-code deploy --all --wait \
   --config-file .config/puppet-code.conf \
   --token-file .config/puppetlabs/token
   ```

1. Update the OpsWorks for Puppet Enterprise server's classification\. By default, the Puppet agent runs on nodes \(including the master\) every 30 minutes\. To avoid waiting, you can manually run the agent on the Puppet master\. Running the agent picks up the new manifest file\.

   1. Sign in to the Puppet Enterprise console\.

   1. Choose **Classification**\.

   1. Expand **PE Infrastructure**\.

   1. Choose **PE Master**\.

   1. On the **Configuration** tab, enter **profile::codecommit** in **Add new class**\.

      The new class, `profile::codecommit`, might not appear immediately after running `puppet-code deploy`\. Choose **Refresh** on this page if it does not appear\.

   1. Choose **Add class**, and then choose **Commit 1 change**\.

   1. Manually run the Puppet agent on the OpsWorks for Puppet Enterprise server\. Choose **Nodes**, choose your server in the list, choose **Run Puppet**, and then choose **Run**\.

1. In the Puppet Enterprise console, change the repository URL to use SSH instead of HTTPS\. The configuration you perform in these steps is saved during the OpsWorks for Puppet Enterprise backup and restoration process, so you do not need to manually change the repository configuration after maintenance activities\.

   1. Choose **Classification**\.

   1. Expand **PE Infrastructure**\.

   1. Choose **PE Master**\.

   1. On the **Configuration** tab, find the `puppet_enterprise::profile::master` class\.

   1. Choose **Edit** next to the `r10k_remote` parameter\.

   1. Replace the HTTPS URL with the SSH URL for your repository, and then choose **Commit 1 change**\.

   1. Manually run the Puppet agent on the OpsWorks for Puppet Enterprise server\. Choose **Nodes**, choose your server in the list, choose **Run Puppet**, and then choose **Run**\.