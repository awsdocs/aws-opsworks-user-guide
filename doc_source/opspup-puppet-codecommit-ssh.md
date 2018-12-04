# Configure SSH Authentication to AWS CodeCommit<a name="opspup-puppet-codecommit-ssh"></a>

Instead of using HTTPS authentication to AWS CodeCommit, you can configure SSH key pair authentication to your r10k remote control repository.

**This process assumes your AWS OpsWorks for Puppet Enterprise server was created with an HTTPS control repository. This must be completed first, so that the needed configuration can be pushed to the server.**

## Step 1: Create an IAM user with CodeCommit permissions

1. Follow the instructions for [creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the AWS Identity and Access Management documentation.

1. Attach the `AWSCodeCommitReadOnly` managed policy to the user.

## Step 2: Create and Associate an SSH Key with Your IAM User

1. Follow the instructions for creating a public/private key pair with `ssh-keygen` in [Step 3: Configure Credentials for Linux, macOS, or Unix](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html#setting-up-ssh-unixes-keys) from the CodeCommit documentation.

1. Follow the instructions for associating your newly created public SSH key with the IAM user you created in [Step 1: Associate Your Public Key with Your IAM User](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-without-cli.html#setting-up-without-cli-add-key) from the CodeCommit documentation.

## Step 3: Upload the Private Key File to AWS Systems Manager Parameter Store

Use the below command to upload the private key file contents to SSM Parameter Store. Your OpsWorks for Puppet Enterprise server will query this parameter to get the needed certificate file.

```bash
aws ssm put-parameter --name puppet_user_pk --type String --value "`cat PRIVATE_KEY_FILE`"
```

## Step 4: Add AWS Systems Manager Parameter Store Permissions to Your OpsWorks for Puppet Enterprise Server

1. Open the *AWS Identity and Access Management (IAM) console*.

1. In the left navigation menu, select **Roles**.

1. Select the role named **aws-opsworks-cm-ec2-role**.

1. In the *Permissions* tab, select **Attach policies**.

1. In the search bar, enter *AmazonEC2RoleforSSM*.

1. Select the checkbox next to the role, *AmazonEC2RoleforSSM*.

1. Select **Attach policy**.

## Step 5: Create the Configuration File Manifest

If you are using the `control-repo-example` repository provided in the starter kit, create the below files in the listed locations. Otherwise, create them according to your control repository structure.

Make sure to replace the **IAM_USER_SSH_KEY** value with the actual SSH key ID you created in Step 2.

`control-repo-example/site/profile/manifests/codecommit.pp`

```ruby
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

## Step 6: Push Your Control Repository to AWS CodeCommit

Run the below commands to push the new manifest file to your repository.

```bash
git add ./site/profile/manifests/codecommit.pp
git commit -m 'Configuring for SSH connection to CodeCommit'
git push origin production
```

## Step 7: Deploy the Manifest Files

Run the below commands to deploy the updated configuration to your OpsWorks for Puppet Enterprise server.

```bash
cd [STARTER_KIT_DIRECTORY]

puppet-access login --config-file .config/puppetlabs/client-tools/puppet-access.conf

puppet-code deploy --all --wait \
--config-file .config/puppet-code.conf \
--token-file .config/puppetlabs/token
```

## Step 8: Update the OpsWorks for Puppet Enterprise Server's Classification

By default, the Puppet Agent will run on nodes (including the master) every 30 minutes. To avoid waiting, you can run Puppet manually on the OpsWorks for Puppet Enterprise server. This will pick up the new manifest file that you created.

1. Log into the Puppet Enterprise console for your server.

1. Select **Classification**.

1. Expand **PE Infrastructure**.

1. Select **PE Master**.

1. Select the **Configuration** tab.

1. In the **Add new class** field, enter `profile::codecommit`.

    The new class, `profile::codecommit`, may not appear immediately after running `puppet-code deploy`. Click the **Refresh** link on this page if it does not appear.

1. Select **Add class**.

1. Select **Commit 1 change**.

    At this point, you need to re-run Puppet Agent on the OpsWorks for Puppet Enterprise server.

1. Select **Nodes**.

1. In the node list, select your OpsWorks for Puppet Enterprise server.

1. Select **Run Puppet...**.

1. Select **Run**.

## Step 9: Change the Repository URL

Now that the needed configuration is in place, the repository URL can be changed to use SSH instead of HTTPS. The configuration performed in these steps is retained as part of the OpsWorks for Puppet Enterprise backup and restore process, so there is no need to manually update this after maintenance activities.

1. Log into the Puppet Enterprise console for your server.

1. Select **Classification**.

1. Expand **PE Infrastructure**.

1. Select **PE Master**.

1. Select the **Configuration** tab.

1. Locate the *puppet_enterprise::profile::master* class.

1. Select **Edit** next to the *r10k_remote* parameter.

1. Replace the HTTPS URL with the SSH URL for your repository.

1. Select **Commit 1 change**.

    At this point, you need to re-run Puppet Agent on the OpsWorks for Puppet Enterprise server.

1. Select **Nodes**.

1. In the node list, select your OpsWorks for Puppet Enterprise server.

1. Select **Run Puppet...**.

1. Select **Run**.
