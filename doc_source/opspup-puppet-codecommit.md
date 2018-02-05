# Optional: Use AWS CodeCommit as a Puppet r10k Remote Control Repository<a name="opspup-puppet-codecommit"></a>

You can create a new repository by using AWS CodeCommit, and use it as your r10k remote control repository\.

1. In the AWS CodeCommit console, create a new repository\.  
![\[Creating new repository in AWS CodeCommit.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_cc_remote.png)

1. Choose **Skip** to skip setting up an Amazon SNS topic\.

1. On the **Code** page, choose **Connect to your repository**\.

1. On the **Connect to your repository** page, choose **HTTPS** as the **Connection type**, and choose your operating system\.  
![\[Creating a new repository in AWS CodeCommit.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_cc_connect.png)

   In the **Steps to clone your repository** area, your `git clone` URL should resemble the following: `https://git-codecommit.region.amazonaws.com/v1/repos/control-repo`\. Copy this URL to a convenient place for use in Puppet server setup\.

1. Close the **Connect to your repository** page, and return to the AWS OpsWorks for Puppet Enterprise server setup\.

1. Paste the URL that you copied in Step 4 in the **r10k remote** string box in the **Configure credentials** page of the Puppet master setup wizard\. Leave the **r10k private key** box empty\. Finish creating and launching your Puppet master\.

1. In the IAM console, attach the **AWSCodeCommitReadOnly** policy to the instance profile role of your Puppet master\. For more information about how to attach a policy to an IAM role, see [Attaching Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console) in the *IAM User Guide*\.

1. Follow the steps in [Setup for HTTPS Users Using Git Credentials](http://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-gc.html) in the *AWS CodeCommit User Guide* to push your existing `control-repo` content to the new AWS CodeCommit repository\.

1. Now, you can continue by following the instructions in [[ERROR] BAD/MISSING LINK TEXT](opspup-starterkit.md), and use the Starter Kit to deploy code to your Puppet master\. The following command is an example\.

   ```
   puppet-code deploy --all --wait --config-file .config/puppet-code.conf
   ```