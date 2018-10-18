# Sign in to the Chef Automate dashboard<a name="opscm-chef-dashboard"></a>

After you have downloaded the sign\-in credentials from the Chef server's Properties page, and the server is online, sign in to the Chef Automate dashboard\. In this walkthrough, we instructed you to first upload cookbooks and add at least one node to manage\. This allows you to see information about the cookbooks and nodes in the dashboard\.

When you attempt to connect to the dashboard webpage, certificate warnings appear in your browser until you install an AWS OpsWorks\-specific, CA\-signed SSL certificate on the client computer that you are using to manage your Chef server\. If you prefer not to see the warnings before you continue to the dashboard webpage, install the SSL certificate before you sign in\.

**To install the AWS OpsWorks SSL certificate**
+ Choose the certificate that matches your system\.
  + For Linux or MacOS\-based systems, download the file with the **PEM** filename extension from the following Amazon S3 location: [https://s3\.amazonaws\.com/opsworks\-cm\-us\-east\-1\-prod\-default\-assets/misc/opsworks\-cm\-ca\-2016\-root\.pem](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2016-root.pem)\.

    For more information about how to install an SSL certificate on MacOS, see [If your certificate isn’t being accepted](https://support.apple.com/kb/PH18677?locale=en_US) on the Apple Support website\.
  + For Windows\-based systems, download the file with the **P7B** filename extension from the following Amazon S3 location: [https://s3\.amazonaws\.com/opsworks\-cm\-us\-east\-1\-prod\-default\-assets/misc/opsworks\-cm\-ca\-2016\-root\.p7b](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-ca-2016-root.p7b)\.

    For more information about how to install an SSL certificate on Windows, see [Manage Trusted Root Certificates](https://technet.microsoft.com/en-us/library/cc754841.aspx) on Microsoft TechNet\.

After you have installed the client\-side SSL certificate, you can sign in to the Chef Automate dashboard without seeing warning messages\.

**Note**  
Users of Google Chrome on Ubuntu and Linux Mint operating systems can have difficulty signing in\. We recommend that you use Mozilla Firefox or other browsers to sign in and use the Chef Automate dashboard on those operating systems\. No issues have been found using Google Chrome on Windows or MacOS\.

**To sign in to the Chef Automate dashboard**

1. Unzip and open the Chef Automate credentials that you downloaded in [Prerequisites](opscm-starterkit.md#finish-server-prereqs)\. You will need these credentials to sign in\.

1. Open the **Properties** page for your Chef server\.

1. At the upper right of the **Properties** page, choose **Open Chef Automate dashboard**\.

1. Sign in using the credentials from Step 1\.  
![\[Chef Automate dashboard sign-in page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_chefsignin.png)

1. In the Chef Automate dashboard, you can view detailed information about the nodes you've bootstrapped, cookbook run progress and events, the compliance level of nodes, and much more\. For more information about the features of the Chef Automate dashboard and how to use them, see the [Chef Automate Documentation](https://docs.chef.io/chef_automate.html)\.  
![\[Chef Automate dashboard\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_chefdashhome.png)

**Note**  
For information about how to change the password that you use to sign in to the Chef Automate dashboard, see [Reset Chef Automate Dashboard Credentials](opscm-resetchefcreds.md)\.