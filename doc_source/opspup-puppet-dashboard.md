# Sign in to the Puppet Enterprise Console<a name="opspup-puppet-dashboard"></a>

After you have downloaded the sign\-in credentials from the Puppet master's Properties page, and the server is online, sign in to the Puppet Enterprise console\. In this walkthrough, we instructed you to specify your control repository that contains your modules, and add at least one node to manage\. This allows you to see information about the agent and nodes in the console\.

When you attempt to connect to the Puppet Enterprise console webpage, certificate warnings appear in your browser until you install an AWS OpsWorks\-specific, CA\-signed SSL certificate on the client computer that you are using to manage your Puppet server\. If you prefer not to see the warnings before you continue to the dashboard webpage, install the SSL certificate before you sign in\.

**To install the AWS OpsWorks SSL certificate**
+ Choose the certificate that matches your system\.
  + For Linux or MacOS\-based systems, download the file with the **PEM** filename extension from the following Amazon S3 location: [https://s3\.amazonaws\.com/opsworks\-cm\-us\-east\-1\-prod\-default\-assets/misc/opsworks\-cm\-ca\-2016\-root\.pem](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2016-root.pem)\.
**Note**  
Additionally, download a newer PEM file from the following location: [https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2020-root.pem](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2020-root.pem)\. Because OpsWorks for Puppet Enterprise is currently renewing its root certificates, you must trust both old and new certificates\.

    For more information about how to manage SSL certificates on MacOS, see [Get information about a certificate in Keychain Access on Mac](https://support.apple.com/guide/keychain-access/get-information-about-a-certificate-kyca15178/11.0/mac/11.0) on the Apple Support website\.
  + For Windows\-based systems, download the file with the **P7B** filename extension from the following Amazon S3 location: [https://s3\.amazonaws\.com/opsworks\-cm\-us\-east\-1\-prod\-default\-assets/misc/opsworks\-cm\-ca\-2016\-root\.p7b](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2016-root.p7b)\.

    For more information about how to install an SSL certificate on Windows, see [Manage Trusted Root Certificates](https://technet.microsoft.com/en-us/library/cc754841.aspx) on Microsoft TechNet\.
**Note**  
Additionally, download a newer P7B file from the following location: [https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2020-root.p7b](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2020-root.p7b)\. Because OpsWorks for Puppet Enterprise is currently renewing its root certificates, you must trust both old and new certificates\.

After you have installed the client\-side SSL certificate, you can sign in to the Puppet Enterprise console without seeing warning messages\.

**To sign in to the Puppet Enterprise console**

1. Unzip and open the Puppet Enterprise credentials that you downloaded in [Prerequisites](opspup-starterkit.md#finish-server-prereqs-puppet)\. You will need these credentials to sign in\.

1. In the AWS Management Console, open the **Properties** page for your Puppet server\.

1. At the upper right of the **Properties** page, choose **Open Puppet Enterprise console**\.  
![\[Puppet master Properties page link to Puppet console\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_link_open_pupconsole.png)

1. Sign in using the credentials from Step 1\.  
![\[Puppet console sign-in page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/puppet_signin_page.png)

1. In the Puppet Enterprise console, you can view detailed information about the nodes you're managing, module run progress and events, the compliance level of nodes, and much more\. For more information about the features of the Puppet Enterprise console and how to use them, see [Managing nodes](https://puppet.com/docs/pe/2019.8/managing_nodes.html) in the Puppet Enterprise documentation\.  
![\[Puppet Enterprise console\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/puppet_console_overview.png)

## Group and Classify Nodes<a name="w100ab1b7c19c17c13"></a>

Before you specify the desired configuration of your nodes by applying classes to them, group the nodes according to their roles in your enterprise or their common characteristics\. Grouping and classifying nodes involves the following high\-level tasks\. You can complete these tasks by using the PE console\. For detailed information about how to group and classify your nodes, see [Grouping and classifying nodes](https://puppet.com/docs/pe/2019.8/grouping_and_classifying_nodes.html) in the Puppet Enterprise documentation\.

1. Create node groups\.

1. Add nodes to groups manually or automatically by applying rules that you create\.

1. Assign classes to node groups\.

## Reset Administrator and User Passwords<a name="w100ab1b7c19c17c15"></a>

For information about how to change the password that you use to sign in to the Puppet Enterprise console, see [Reset the console administrator password](https://puppet.com/docs/pe/2019.8/console_accessing.html#reset_the_admin_password) in the Puppet Enterprise documentation\.

By default, after ten sign\-in attempts, users are locked out of the Puppet console\. For more information about how to reset user passwords in the event of a lockout, see [Password endpoints](https://puppet.com/docs/pe/2019.8/rbac_api_v1_password.html#post_users_sid_password_reset) in the Puppet Enterprise documentation\.