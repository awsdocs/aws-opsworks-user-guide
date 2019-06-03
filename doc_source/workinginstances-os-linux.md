# Linux Operating Systems<a name="workinginstances-os-linux"></a>

AWS OpsWorks Stacks supports the 64\-bit versions of the following Linux operating systems\.
+ [Amazon Linux](http://aws.amazon.com/amazon-linux-ami/faqs/) \(see the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) for the currently supported versions\)
+ [Ubuntu 12\.04 LTS](https://wiki.ubuntu.com/LTS)
+ [Ubuntu 14\.04 LTS](https://wiki.ubuntu.com/LTS)
+ [Ubuntu 16\.04 LTS](https://wiki.ubuntu.com/LTS)
+ [Ubuntu 18\.04 LTS](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes)
+ [CentOS 7](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7)
+ [Red Hat Enterprise Linux 7](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/)

You can also use [custom AMIs](workinginstances-custom-ami.md) based on these operating systems\. 

Some general notes on Linux instances:

**Supported Package Versions**  
The supported versions and patch levels for packages, such as Ruby, depend on the operating system and version as described in the following sections\. 

**Updates**  
By default, AWS OpsWorks Stacks ensures that Linux instances have the latest security patches by automatically calling `yum update` or `apt-get update` after an instance boots\. To disable automatic updates use the [CreateInstance](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateInstance.html), [UpdateInstance](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_UpdateInstance.html), [CreateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_CreateLayer.html), or [UpdateLayer](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_UpdateLayer.html) actions—or the equivalent [AWS SDK](https://aws.amazon.com/tools/) methods or [AWS CLI](http://aws.amazon.com/documentation/cli/) commands— to set the `InstallUpdatesOnBoot` parameter to `false`\.  
To avoid service interruptions, AWS OpsWorks Stacks does not automatically install updates after an instance is online\. You can manually update an online instance's operating system at any time by running the [Upgrade Operating System stack command](workingstacks-commands.md)\. For more information on how to manage security updates, see [Managing Security Updates](workingsecurity-updates.md)\.  
For more control over how AWS OpsWorks Stacks updates your instances, create a custom AMI based on one of the supported operating systems\. For example, with custom AMIs you can specify which package versions are installed on an instance\. Each Linux distribution has different support timelines and package\-merge policies, so you should consider which approach best suits your requirements\. For more information, see [Using Custom AMIs](workinginstances-custom-ami.md)\.

**Hosts File**  
Each online instance has a `/etc/hosts` file that maps IP addresses to host names\. AWS OpsWorks Stacks includes the public and private addresses for all of the stack's online instances in each instance's `hosts` file\. For example, suppose that you have a stack with two Node\.js App Server instances, nodejs\-app1 and nodejs\-app2, and one MySQL instance, db\-master1\. The nodejs\-app1 instance's `hosts` file will look something like the following example, and the other instances' will have similar `hosts` files\.  

```
...
# OpsWorks Layer State
192.0.2.0 nodejs-app1.localdomain nodejs-app1
10.145.160.232 db-master1
198.51.100.0 db-master1-ext
10.243.77.78 nodejs-app2
203.0.113.0 nodejs-app2-ext
10.84.66.6 nodejs-app1
192.0.2.0 nodejs-app1-ext
```

**AWS OpsWorks Stacks Agent Proxy Support**  
The AWS OpsWorks Stacks agent for Chef 11\.10 and later stacks includes basic support for proxy servers, which are typically used with isolated VPCs\. To enable proxy server support, an instance must have an `/etc/environment` file that provides the appropriate settings for HTTP and HTTPS traffic\. The file should look similar to the following, where you replace the highlighted text with your proxy server's URL and port:  

```
http_proxy="http://myproxy.example.com:8080/"
https_proxy="http://myproxy.example.com:8080/"
no_proxy="169.254.169.254"
```
To enable proxy support, we recommend [creating a custom AMI](workinginstances-custom-ami.md) that includes an appropriate `/etc/environment` file and using that AMI to create your instances\.   
We do not recommend using a custom recipe to create an `/etc/environment` file on your instances\. AWS OpsWorks Stacks needs the proxy server data early in the setup process, before any custom recipes have executed\.

**Topics**
+ [Amazon Linux](#workinginstances-os-amazon)
+ [Ubuntu LTS](#workinginstances-os-linux-ubuntu)
+ [CentOS](#workinginstances-os-linux-centos)
+ [Red Hat Enterprise Linux](#workinginstances-os-linux-rhel)

## Amazon Linux<a name="workinginstances-os-amazon"></a>

AWS OpsWorks Stacks supports the 64\-bit version of Amazon Linux\. In addition to regular updates and patches, Amazon Linux releases a new version approximately every six months, which can involve significant changes\. For example, the 2012\.09 to 2013\.03 update upgraded the kernel from 3\.2 to 3\.4\. When you create a stack or a new instance, you must specify which Amazon Linux version to use\. When AWS releases a new version, your instances continue to run the specified version until you explicitly change it\. After a new Amazon Linux version is released, there is a four\-week migration period, during which AWS continues to provide regular updates for the old version\. After the migration period ends, your instances can continue to run the old version, but AWS does not provide further updates\. For more information, see [Amazon Linux AMI FAQs](http://aws.amazon.com/amazon-linux-ami/faqs/#lock)\.

When a new Amazon Linux version is released, we recommend that you update to the new version within the migration period so your instances continue to receive security updates\. Before updating your production stack's instances, we recommend you start a new instance and verify that your app runs correctly on the new version\. You can then update the production stack instances\.

**Note**  
By default, custom AMIs based on Amazon Linux are automatically updated to the new version when it is released\. The recommended practice is to lock your custom AMI to a specific Amazon Linux version so you can defer the update until you have tested the new version\. For more information, see [How do I lock my AMI to a specific version?](http://aws.amazon.com/amazon-linux-ami/faqs/#lock)\.  
If you use an AWS CloudFormation template to create stacks with instances running Amazon Linux, the templates should explicitly specify an Amazon Linux version\. In particular, if your template specifies `Amazon Linux`, the instances will continue to run version 2016\.09\. For more information, see [AWS::OpsWorks::Stack](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-opsworks-stack.html) and [AWS::OpsWorks::Instance](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-opsworks-instance.html)\.

To update an instance's Amazon Linux version, do one of the following:
+ For online instances, run the [**Upgrade Operating System** stack command](workingstacks-commands.md)\.

  When a new Amazon Linux version is available, the **Instances** and **Stack** pages display a notice with a link that takes you to the **Run Command** page\. You can then run **Upgrade Operating System** to upgrade your instance\.
+ For offline Amazon Elastic Block Store\-backed \(EBS\-backed\) instances, start the instances and run **Upgrade Operating System**, as described in the preceding bullet item\.
+ For offline instance store\-backed instances, including time\-based and load\-based instances, [edit the instance's **Operating system** setting](workinginstances-properties.md) to specify the new version\.

  AWS OpsWorks Stacks automatically updates the instances to the new version when they are restarted\.


**Amazon Linux: Supported Node\.js Versions**  

| Amazon Linux Version | Node\.js Versions | 
| --- | --- | 
|   <pre>2author="gabykap" timestamp="20160930T124501-0700"</pre>   |  <pre>(Not applicable to operating systems that are available for Chef 12 and higher stacks only)</pre>  | 
|   <pre>2018author="gabykap" timestamp="20160930T124501-0700".03author="gabykap" timestamp="20160930T124501-0700"</pre>   |   <pre>0.12.18<br />0.12.17<br />0.12.16<br />0.12.15<br />0.12.14<br />0.12.13<br />0.12.12<br />0.12.10<br />0.10.40author="gabykap" timestamp="20160930T124501-0700"</pre>   | 
|  <pre>2017.09</pre>  |    <pre>0.12.18<br />0.12.17<br />0.12.16<br />0.12.15<br />0.12.14<br />0.12.13<br />0.12.12<br />0.12.10<br />0.10.40author="gabykap" timestamp="20160930T124501-0700"</pre>    | 
|  <pre>2017.03</pre>  |    <pre>0.12.18<br />0.12.17<br />0.12.16<br />0.12.15<br />0.12.14<br />0.12.13<br />0.12.12<br />0.12.10<br />0.10.40author="gabykap" timestamp="20160930T124501-0700"</pre>    | 
|  <pre>2016.09</pre>  |  <pre>0.12.18<br />0.12.17<br />0.12.16<br />author="gabykap" timestamp="20160930T124501-0700"0.12.15</pre>  | 
|  <pre>2016.03</pre>  |  <pre>0.12.18<br />0.12.17<br />0.12.16<br />0.12.15<br />0.12.13<br />0.12.12<br />0.12.10</pre>  | 
|  <pre>2015.09</pre>  |  <pre>0.12.15<br />0.12.13<br />0.12.12<br />0.12.10<br />0.12.7<br />0.10.40</pre>  | 
|  <pre>2015.03</pre>  |  <pre>0.12.15<br />0.12.13<br />0.12.12<br />0.12.10<br />0.12.7<br />0.10.40</pre>  | 


**Amazon Linux: Supported Chef Versions**  

| Chef Version | Supported Amazon Linux Versions | 
| --- | --- | 
|  <pre>12</pre>  |  <pre>Amazon Linux 2<br />Amazon Linux 2018.03<br />Amazon Linux 2017.09<br />Amazon Linux 2017.03<br />Amazon Linux 2016.09<br />Amazon Linux 2016.03</pre>  | 
|  <pre>11.10</pre>  |  <pre>Amazon Linux 2018.03<br />Amazon Linux 2017.09<br />Amazon Linux 2017.03<br />Amazon Linux 2016.09<br />Amazon Linux 2016.03<br />Amazon Linux 2015.09<br />Amazon Linux 2015.03<br />Amazon Linux 2014.09 (deprecated)<br />Amazon Linux 2014.03 (deprecated)</pre>  | 
|  <pre>11.4 (deprecated)</pre>  |  <pre>Amazon Linux 2016.09<br />Amazon Linux 2016.03<br />Amazon Linux 2015.09<br />Amazon Linux 2015.03<br />Amazon Linux 2014.09 (deprecated)<br />Amazon Linux 2014.03 (deprecated)</pre>  | 

**Important**  
Before updating t1\.micro instances, make sure they have a temporary swap file, `/var/swapfile`\. The t1\.micro instances on Chef 0\.9 stacks do not have a swap file\. For Chef 11\.4 and Chef 11\.10 stacks, recent versions of the instance agent automatically create a swap file for t1\.micro instances\. However, this change was introduced over a period of several weeks, so you should check for the existence of `/var/swapfile` on instances created before approximately Mar\. 24, 2014\.   
For t1\.micro instances that lack a swap file, you can create one as follows:   
For Chef 11\.10 and later stacks, create new t1\.micro instances, which automatically have a swap file\. 
For Chef 0\.9 stacks, run the following commands on each instance as root user\.  

  ```
  dd if=/dev/zero of=/var/swapfile bs=1M count=256
   mkswap /var/swapfile
   chown root:root /var/swapfile
   chmod 0600 /var/swapfile
   swapon /var/swapfile
  ```
You can also use these commands on Chef 11\.10 and later stacks if you don't want to create new instances\.

## Ubuntu LTS<a name="workinginstances-os-linux-ubuntu"></a>

Ubuntu releases a new Ubuntu LTS version approximately every two years and supports each release for approximately five years\. Ubuntu provides security patches and updates for the duration of the operating system support\. For more information, see [LTS \- Ubuntu Wiki](https://wiki.ubuntu.com/LTS)\.

AWS OpsWorks Stacks supports the 64\-bit versions of Ubuntu 12\.04 LTS and Ubuntu 14\.04 LTS\.
+ Ubuntu 12\.04 is supported for all stacks\.
+ Ubuntu 14\.04 is supported only for Chef 11\.10 and higher stacks\.
+ You cannot update an existing Ubuntu instance to a newer release of Ubuntu\.

  You must [create a new Ubuntu 14\.04, 16\.04, or 18\.04 instance](workinginstances-add.md) and [delete the older instance](workinginstances-delete.md)\.
+ Ubuntu 16\.04 LTS and 18\.04 LTS are supported only for Chef 12 and higher stacks\.


**Ubuntu: Supported Node\.js Versions**  

| Ubuntu Version | Node\.js Versions | 
| --- | --- | 
|  <pre>18.04 LTS</pre>  |  <pre>(Not applicable to operating systems that are available for Chef 12 and higher stacks only)</pre>  | 
|  <pre>16.04 LTS</pre>  |  <pre>(Not applicable to operating systems that are available for Chef 12 and higher stacks only)</pre>  | 
|  <pre>14.04 LTS</pre>  |  <pre>0.10.27<br />0.10.29<br />0.10.40<br />0.12.10<br />0.12.12<br />0.12.13<br />0.12.15</pre>  | 
|  <pre>12.04 LTS</pre>  |  <pre>0.8.19<br />0.8.26<br />0.10.11<br />0.10.21<br />0.10.24<br />0.10.25<br />0.10.27<br />0.10.29<br />0.10.40<br />0.12.10<br />0.12.12<br />0.12.13<br />0.12.15</pre>  | 


**Ubuntu: Supported Chef Versions**  

| Chef Version | Supported Ubuntu Versions | 
| --- | --- | 
|  <pre>12</pre>  |  <pre>Ubuntu 18.04 LTS<br />Ubuntu 16.04 LTS<br />Ubuntu 14.04 LTS<br />Ubuntu 12.04 LTS</pre>  | 
|  <pre>11.10</pre>  |  <pre>Ubuntu 14.04 LTS<br />Ubuntu 12.04 LTS</pre>  | 
|  <pre>11.4 (deprecated)</pre>  |  <pre>Ubuntu 12.04 LTS</pre>  | 

All versions of Node\.js that are older than 0\.10\.40 are deprecated\. 0\.12\.7 and 0\.12\.9 are also deprecated\.

## CentOS<a name="workinginstances-os-linux-centos"></a>

AWS OpsWorks Stacks supports the 64\-bit version of [CentOS 7](https://wiki.centos.org/FrontPage)\. The initial supported version is CentOS 7, and CentOS releases a new version approximately every two years\. For more information, see [Questions about CentOS 7](https://wiki.centos.org/FAQ/CentOS7)\.

When you start a new instance in a CentOS stack, AWS OpsWorks Stacks automatically installs the most current CentOS version\. Because AWS OpsWorks Stacks does not automatically update the operating system on existing instances when a new CentOS minor version is released, a newly created instance might receive a more recent version than the stack's existing instances\. To keep versions consistent across your stack, you can update your existing instances to the current CentOS version, as follows:
+ For online instances, run the [**Upgrade Operating System** stack command](workingstacks-commands.md), which runs `yum update` on the specified instances to update them to the current version\.

  When a new CentOS 7 minor version is available, the **Instances** and **Stack** pages display a notice with a link that takes you to the **Run Command** page\. You can then run **Upgrade Operating System** to upgrade your instances\.
+ For offline Amazon EBS\-backed instances, start the instances and run **Upgrade Operating System** as described in the preceding list item\.
+ For offline instance store\-backed instances, AWS OpsWorks Stacks automatically installs the new version when the instances are restarted\.


**CentOS: Supported Chef Versions**  

| Chef Version | Supported CentOS Version | 
| --- | --- | 
|  <pre>12</pre>  |  <pre>CentOS 7</pre>  | 
|  <pre>11.10</pre>  |  <pre>(None supported)</pre>  | 
|  <pre>11.4 (deprecated)</pre>  |  <pre>(None supported)</pre>  | 

**Note**  
AWS OpsWorks Stacks supports Apache 2\.4 for CentOS instances\.

## Red Hat Enterprise Linux<a name="workinginstances-os-linux-rhel"></a>

AWS OpsWorks Stacks supports the 64\-bit version of [Red Hat Enterprise Linux 7](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/) \(RHEL 7\)\. The initial supported version is RHEL 7\.1 and Red Hat releases a new minor version approximately every 9 months\. Minor versions should be compatible with RHEL 7\.0\. For more information, see [Life Cycle and Update Policies](https://access.redhat.com/support/policy/update_policies)\.

When you start a new instance, AWS OpsWorks Stacks automatically installs the current RHEL 7 version\. Because AWS OpsWorks Stacks does not automatically update the operating system on existing instances when a new RHEL 7 minor version is released, a newly created instance might receive a more recent version than the stack's existing instances\. To keep versions consistent across your stack, you can update your existing instances to the current RHEL 7 version, as follows:
+ For online instances, run the [**Upgrade Operating System** stack command](workingstacks-commands.md), which runs `yum update` on the specified instances to update them to the current version\.

  When a new RHEL 7 version is available, the **Instances** and **Stack** pages display a notice with a link that takes you to the **Run Command** page\. You can then run **Upgrade Operating System** to upgrade your instances\.
+ For offline Amazon EBS\-backed instances, start the instances and run **Upgrade Operating System** as described in the preceding list item\.
+ For offline instance store\-backed instances, AWS OpsWorks Stacks automatically installs the new version when the instances are restarted\.


**Red Hat Enterprise Linux: Supported Node\.js Versions**  

| RHEL Version | Node\.js Versions | 
| --- | --- | 
|  <pre>7</pre>  |  <pre>(Node.js versions only apply to Chef 11.10 stacks)<br />0.8.19<br />0.8.26<br />0.10.11<br />0.10.21<br />0.10.24<br />0.10.25<br />0.10.27<br />0.10.29<br />0.10.40<br />0.12.10<br />0.12.12<br />0.12.13<br />0.12.15</pre>  | 


**Red Hat Enterprise Linux: Supported Chef Versions**  

| Chef Version | Supported RHEL Version | 
| --- | --- | 
|  <pre>12</pre>  |  <pre>Red Hat Enterprise Linux 7</pre>  | 
|  <pre>11.10</pre>  |  <pre>Red Hat Enterprise Linux 7</pre>  | 
|  <pre>11.4 (deprecated)</pre>  |  <pre>(None supported)</pre>  | 

All versions of Node\.js that are older than 0\.10\.40 are deprecated\. 0\.12\.7 and 0\.12\.9 are also deprecated\.

**Note**  
AWS OpsWorks Stacks supports Apache 2\.4 for RHEL 7 instances\.