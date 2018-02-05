# Chef Compliance in AWS OpsWorks for Chef Automate<a name="opscm-chefcompliance"></a>

[Chef Compliance](https://www.chef.io/solutions/compliance/) lets you track the compliance of managed nodes in your infrastructure based on predefined policies, also called *rules*\. Compliance views let you regularly audit your applications for vulnerabilities and noncompliant configurations\. Chef offers dozens of predefined Compliance *profiles*—collections of rules that apply to specific node configurations—that you can use in your Compliance scans\. You can also use the [Chef Compliance language](https://docs.chef.io/chef_compliance.html) to create your own custom profiles \.

**Note**  
To use Chef Compliance, you must have version 12\.16\.42 or later of the `chef-client` agent installed on your managed nodes\.

## Setting Up Chef Compliance<a name="opscm-chefcompliance-setup"></a>

You can configure Chef Compliance on any AWS OpsWorks for Chef Automate server\. After you launch an AWS OpsWorks for Chef Automate server, choose profiles that you want to run from the profiles in the Chef Automate dashboard\. After you install profiles, run `berks` commands to upload the [Audit cookbook](https://supermarket.chef.io/cookbooks/audit) to your Chef Automate server\. Installing the Audit cookbook also installs the gem for [InSpec](https://www.inspec.io/), an open\-source testing framework produced by Chef that lets you integrate automated tests into any stage of your deployment pipeline\.

Because the current version of Chef Automate is 1\.6\.x, choose version 4\.0\.0 or later of the Audit cookbook\. The InSpec gem must be version 1\.24\.0 or later\.

After you install the Audit cookbook, add the Audit recipe to the run list of your managed nodes\.

**To install Compliance profiles**

1. If you have not already done so, sign in to the Chef Automate web\-based dashboard\. Use the credentials you received when you downloaded the Starter Kit as you created your AWS OpsWorks for Chef Automate server\.

1. In the Chef Automate dashboard, choose the **Compliance** tab\.  
![\[Chef Compliance profiles\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_compliance_profiles.png)

1. In the left navigation bar, choose **Profile Store**, and then choose the **Available** tab to see predefined profiles\.

1. Browse the list of profiles\. Choose a profile that matches the operating system and configuration of at least one of your managed nodes\. To view details about the profile, including a description of the profile's targeted violations and underlying rule code, choose **>** at the right of the profile entry\. You can choose multiple profiles\.  
![\[Chef Compliance profile detail view\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_compliance_rule.png)

1. To install the selected profiles on your Chef Automate server, choose **Get**\.

1. When the download is complete, go to the next procedure, [To install the Chef Compliance Audit cookbook](#compliance-installaudit)\.

**To install the Chef Compliance Audit cookbook**

1. In a text editor, append the [Audit cookbook](https://supermarket.chef.io/cookbooks/audit) to your Berksfile\. Add the following lines at the end of your existing list of cookbooks\. Save and close your Berksfile\.

   ```
   source 'https://supermarket.chef.io
   cookbook 'audit', '4.0.0'
   ```

1. Download and install the cookbook on your local computer\.

   ```
   berks install
   ```

1. Upload the cookbook to the Chef server\.

   On Linux, run the following\. The `pem` file is an SSL private key signed by a certification authority \(CA\) and provided by AWS OpsWorks\. This key allows the server to identify itself to the `chef-client` agent on nodes that your server manages\.

   ```
   SSL_CERT_FILE='.chef/ca_certs/opsworks-cm-ca-2016-root.pem' berks upload
   ```

   On Windows, run the following Chef DK command in a PowerShell session\. Before you run the command, be sure to set the execution policy in PowerShell to `RemoteSigned`\. Add `chef shell-init` to make Chef DK utility commands available to PowerShell\.

   ```
   $env:SSL_CERT_FILE="ca_certs\opsworks-cm-ca-2016-root.pem"
   chef shell-init berks upload
   Remove-Item Env:\SSL_CERT_FILE
   ```

1. To verify the installation of the cookbook, run the following command to show a list of cookbooks that are currently available on the Chef Automate server\. The Audit cookbook should be in the list of results\.

   ```
   knife cookbook list
   ```

1. Go to the next procedure, [To configure nodes for the Audit cookbook](#compliance-confignodes)\.

**To configure nodes for the Audit cookbook**

For more information about how to configure node run lists and add attributes to the Audit cookbook, see [Sending Compliance Data to Chef Automate with Audit Cookbook](https://docs.chef.io/audit_cookbook.html) in the Chef documentation and the [Audit cookbook readme](https://supermarket.chef.io/cookbooks/audit#readme) in the Chef Supermarket\.

+ Add the Audit cookbook recipe to the run list of each node on which you want to perform a Compliance scan\. You add Compliance profiles that you downloaded in [To install Compliance profiles](#compliance-installprof) by adding the `node['audit']['profiles']` attribute to a node's run list\.

## Running a Compliance Scan<a name="opscm-chefcompliance-scan"></a>

By default, the `chef-client` agent runs recipes on nodes every 1800 seconds \(30 minutes\)\. You should see compliance scan results in the Chef Automate dashboard shortly after the first run of the agent daemon that occurs after you configure node run lists\.

![\[Chef Compliance Reporting page view\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_compliance_reporting.png)

In the Chef Automate dashboard, choose the **Compliance** tab\. In the left navigation pane, choose **Reporting**\. Choose the **Profiles** tab, choose **Scan Results**, and then choose a node with scan failures to learn more about the rules against which a node failed\.

## Updating Chef Compliance<a name="opscm-chefcompliance-updates"></a>

On an AWS OpsWorks for Chef Automate server, Chef Compliance versions are updated automatically by your scheduled system maintenance\. As updated releases of Chef Automate and Chef Server become available for your AWS OpsWorks for Chef Automate server, you might need to check and update the supported versions of the Audit cookbook and InSpec gem that are running on your server\. Profiles that you have already installed on your AWS OpsWorks for Chef Automate server are not updated as part of maintenance\.

## Community and Custom Compliance Profiles<a name="opscm-compliance-custom"></a>

Chef currently includes almost 90 Compliance scan profiles\. You can add community and custom profiles to the list, and then download and run Compliance scans based on those profiles, just as you would for included profiles\. For more information, see [Using Community Compliance profiles](https://learn.chef.io/modules/using-community-profiles#/) in the Learn Chef tutorials\. Community\-based Compliance profiles are available from the [Chef Supermarket](https://supermarket.chef.io/tools?q=&type=compliance_profile)\. Custom profiles are Ruby\-based programs that include a folder of controls that specify your scan rules\.

## See Also<a name="opscm-compliance-seealso"></a>

+ [Chef Compliance announcement blog post](https://blog.chef.io/2017/07/05/chef-automate-release-july-2017/)

+ [Compliance documentation](https://docs.chef.io/automate_compliance_reporting.html) on the Chef website

+ [Chef Automate Compliance online training](https://training.chef.io/instructor-led-training/chef-automate-compliance)

+ [InSpec](https://www.inspec.io/) website