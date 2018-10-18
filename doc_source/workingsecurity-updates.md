# Managing Linux Security Updates<a name="workingsecurity-updates"></a>

## Security Updates<a name="bestpractice-secupdates"></a>

Linux operating system providers supply regular updates, most of which are operating system security patches but can also include updates to installed packages\. You should ensure that your instances' operating systems are current with the latest security patches\. 

By default, AWS OpsWorks Stacks automatically installs the latest updates during setup, after an instance finishes booting\. AWS OpsWorks Stacks does not automatically install updates after an instance is online, to avoid interruptions such as restarting application servers\. Instead, you manage updates to your online instances yourself, so you can minimize any disruptions\.

We recommend that you use one of the following to update your online instances\.
+ Create and start new instances to replace your current online instances\. Then delete the current instances\.

  The new instances will have the latest set of security patches installed during setup\.
+ On Linux\-based instances in Chef 11\.10 or older stacks, run the [Update Dependencies stack command](workingstacks-commands.md), which installs the current set of security patches and other updates on the specified instances\.

For both of these approaches, AWS OpsWorks Stacks performs the update by running `yum update` for Amazon Linux and Red Hat Enterprise Linux \(RHEL\) or `apt-get update` for Ubuntu\. Each distribution handles updates somewhat differently, so you should examine the information in the associated links to understand exactly how an update will affect your instances: 
+ **Amazon Linux** – Amazon Linux updates install security patches and might also install feature updates, including package updates\.

  For more information, see [Amazon Linux AMI FAQs](http://aws.amazon.com/amazon-linux-ami/faqs/#lock)\.
+ **Ubuntu** – Ubuntu updates are largely limited to installing security patches, but might also install package updates for a limited number of critical fixes\.

  For more information, see [LTS \- Ubuntu Wiki](https://wiki.ubuntu.com/LTS)\.
+ **CentOS** – CentOS updates generally maintain binary compatibility with earlier versions\.

  For more information, see [CentOS Product Specifications](https://wiki.centos.org/About/Product)\.
+ **RHEL** – RHEL updates generally maintain binary compatibility with earlier versions\.

  For more information, see [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata/)\.

If you want more control over updates, such as specifying particular package versions, you can disable automatic updates by using the [CreateInstance](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateInstance.html), [UpdateInstance](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_UpdateInstance.html), [CreateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateLayer.html), or [UpdateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_UpdateLayer.html) actions—or the equivalent [AWS SDK](https://aws.amazon.com/tools/) methods or [AWS CLI](http://aws.amazon.com/documentation/cli/) commands—to set the `InstallUpdatesOnBoot` parameter to `false`\. The following example shows how to use the AWS CLI to disable `InstallUpdatesOnBoot` as the default setting for an existing layer\.

```
aws opsworks update-layer --layer-id layer ID --no-install-updates-on-boot
```

You must then manage updates yourself\. For example, you could employ one of these strategies:
+ Implement a custom recipe that [runs the appropriate shell command](cookbooks-101-basics-commands.md#cookbooks-101-basics-commands-script) to install your preferred updates\.

  Because system updates don't map naturally to a [lifecycle event](workingcookbook-events.md), include the recipe in your custom cookbooks but [execute it manually](workingcookbook-manual.md)\. For package updates, you can also use the [yum\_package](https://docs.chef.io/chef/resources.html#yum-package) \(Amazon Linux\) or [apt\_package](https://docs.chef.io/chef/resources.html#apt-package) \(Ubuntu\) resources instead of a shell command\. 
+ [Log in to each instance with SSH](workinginstances-ssh.md) and run the appropriate commands manually\.