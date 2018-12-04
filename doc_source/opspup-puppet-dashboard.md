# Sign in to the Puppet Enterprise Console<a name="opspup-puppet-dashboard"></a>

After you have downloaded the sign\-in credentials from the Puppet master's Properties page, and the server is online, sign in to the Puppet Enterprise console\. In this walkthrough, we instructed you to specify your control repository that contains your modules, and add at least one node to manage\. This allows you to see information about the agent and nodes in the console\.

The Puppet Enterprise console uses a certificate signed by PE’s built\-in certificate authority \(CA\)\. Because this CA is specific to PE, your browser might not trust it, and you might have to add a security exception to access the console\. For more information about troubleshooting certificate errors and adding exceptions in common web browsers, see the following\.
+ For Mozilla Firefox, see [Troubleshoot the "Secure Connection Failed" error message](https://support.mozilla.org/en-US/kb/secure-connection-failed-error-message) on the Mozilla support website\.
+ For Google Chrome, see [Browser displays connection untrusted error](https://support.google.com/gsa/answer/2688801?hl=en) on the Google support website\.
+ For Microsoft Internet Explorer, see [Certificate Errors: FAQ](https://support.microsoft.com/en-us/help/17430/windows-internet-explorer-certificate-errors-faq#ie) on the Microsoft support website\.

**To sign in to the Puppet Enterprise console**

1. Unzip and open the Puppet Enterprise credentials that you downloaded in [Prerequisites](opspup-starterkit.md#finish-server-prereqs-puppet)\. You will need these credentials to sign in\.

1. In the AWS Management Console, open the **Properties** page for your Puppet server\.

1. At the upper right of the **Properties** page, choose **Open Puppet Enterprise console**\.  
![\[Puppet master Properties page link to Puppet console\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_link_open_pupconsole.png)

1. Sign in using the credentials from Step 1\.  
![\[Puppet console sign-in page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/puppet_signin_page.png)

1. In the Puppet Enterprise console, you can view detailed information about the nodes you're managing, module run progress and events, the compliance level of nodes, and much more\. For more information about the features of the Puppet Enterprise console and how to use them, see [Viewing node\-specific information](https://docs.puppet.com/pe/2017.2/nodes_viewing.html) in the Puppet Enterprise documentation\.  
![\[Puppet Enterprise console\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/puppet_console_overview.png)

## Group and Classify Nodes<a name="w4ab1b7c19c15c11"></a>

Before you specify the desired configuration of your nodes by applying classes to them, group the nodes according to their roles in your enterprise or their common characteristics\. Grouping and classifying nodes involves the following high\-level tasks\. You can complete these tasks by using the PE console\. For detailed information about how to group and classify your nodes, see [Grouping and classifying nodes](https://puppet.com/docs/pe/2017.3/managing_nodes/grouping_and_classifying_nodes.html) in the Puppet Enterprise documentation\.

1. Create node groups\.

1. Add nodes to groups manually or automatically by applying rules that you create\.

1. Assign classes to node groups\.

## Reset Administrator and User Passwords<a name="w4ab1b7c19c15c13"></a>

For information about how to change the password that you use to sign in to the Puppet Enterprise console, see [Reset the Admin Password](https://puppet.com/docs/pe/2017.3/accessing_console/console_accessing.html#reset-the-admin-password) in the Puppet Enterprise documentation\.

By default, after ten sign\-in attempts, users are locked out of the Puppet console\. For more information about how to reset user passwords in the event of a lockout, see [Password endpoints](https://puppet.com/docs/pe/2017.3/api_rbac_activity/rbac_api_v1_password.html) in the Puppet Enterprise documentation\.