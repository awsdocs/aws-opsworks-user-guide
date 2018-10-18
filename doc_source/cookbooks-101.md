# Cookbooks 101<a name="cookbooks-101"></a>

A production\-level AWS OpsWorks Stacks stack typically requires some [customization](customizing.md), which often means implementing a custom Chef cookbook with one or more recipes, attribute files, or template files\. This topic is a tutorial introduction to implementing cookbooks for AWS OpsWorks Stacks\.

For more information on how AWS OpsWorks Stacks uses cookbooks, which includes a brief general introduction to cookbooks, see [Cookbooks and Recipes](workingcookbook.md)\. For additional information on how to implement and test Chef recipes, see [Test\-Driven Infrastructure with Chef, 2nd Edition](http://www.amazon.com/Test-Driven-Infrastructure-Chef-Behavior-Driven-Development/dp/1449372201/ref=sr_1_fkmr0_1?ie=UTF8&qid=1405556803&sr=8-1-fkmr0&keywords=Test-Driven+Infrastructure+with+Chef%2C+2nd+Edition)\.

The tutorial examples are divided into two sections:
+  [Cookbook Basics](cookbooks-101-basics.md) is a set of example walkthroughs that are intended for users who are not familiar with Chef; experienced Chef users can skip this section\.

  The examples walk you through the basics of how to implement cookbooks to perform common tasks, such as installing packages or creating directories\. To simplify the process, you will use a pair of useful tools, [Vagrant](http://docs.vagrantup.com/v2/) and [Test Kitchen](http://kitchen.ci/), to run most of the examples locally in a virtual machine\. Before starting [Cookbook Basics](cookbooks-101-basics.md), you should first read [Vagrant and Test Kitchen](#cookbooks-101-tools) to learn how to install and use these tools\. Because Test Kitchen does not yet support Windows, the examples are all for Linux, with notes indicating how to adapt them for Windows\.
+ [Implementing Cookbooks for AWS OpsWorks Stacks](cookbooks-101-opsworks.md) describes how to implement recipes for AWS OpsWorks Stacks, including for Windows stacks\.

  It also includes some more advanced topics such as how to use Berkshelf to manage external cookbooks\. The examples are written for new Chef users, much like the examples in [Cookbook Basics](cookbooks-101-basics.md)\. However AWS OpsWorks Stacks works a bit differently than Chef server, so we recommend that experienced Chef users at least read through this section\.

**Topics**
+ [Vagrant and Test Kitchen](#cookbooks-101-tools)
+ [Cookbook Basics](cookbooks-101-basics.md)
+ [Implementing Cookbooks for AWS OpsWorks Stacks](cookbooks-101-opsworks.md)

## Vagrant and Test Kitchen<a name="cookbooks-101-tools"></a>

If you are working with recipes for Linux instances, Vagrant and Test Kitchen are very useful tools for learning and initial development and testing\. This topic provides brief descriptions of Vagrant and Test Kitchen, and points you to installation instructions and walkthroughs that will get you set up and familiarize you with the basics of how to use the tools\. Although Vagrant does support Windows, Test Kitchen does not, so only Linux examples are provided for these tools\.

**Topics**
+ [Vagrant](#cookbooks-101-tools-vagrant)
+ [Test Kitchen](#cookbooks-101-tools-test-kitchen)

### Vagrant<a name="cookbooks-101-tools-vagrant"></a>

[Vagrant](http://docs.vagrantup.com/v2/) provides a consistent environment for executing and testing code on a virtual machine\. It supports a wide variety of environments—called Vagrant boxes—each of which represents a configured operating system\. For AWS OpsWorks Stacks, the environments of interest are based on Ubuntu, Amazon, or Red Hat Enterprise Linux \(RHEL\) distributions, so the examples primarily use a Vagrant box named `opscode-ubuntu-12.04`\. It represents a Ubuntu 12\.04 LTS system that is configured for Chef\.

Vagrant is available for Linux, Windows, and Macintosh systems, so you can use your preferred workstation to implement and test recipes on any supported operating system\. The examples for this chapter were created on an Ubuntu 12\.04 LTS Linux system, but translating the procedures to Windows or Macintosh systems is straightforward\.

Vagrant is basically a wrapper for a virtualization provider\. Most of the examples use the [VirtualBox](https://www.virtualbox.org/) provider\. VirtualBox is free and available for Linux, Windows, and Macintosh systems\. The Vagrant walkthrough provides installation instructions if you do not already have VirtualBox on your system\. Note that you can run Ubuntu\-based environments on VirtualBox, but Amazon Linux is available only for Amazon EC2 instances\. However, you can run a similar operating system such as CentOS on VirtualBox, which is useful for initial development and testing\.

For information on other providers, see the [Vagrant](http://docs.vagrantup.com/v2/) documentation\. In particular, the `vagrant-aws` plug\-in provider allows you to use Vagrant with Amazon EC2 instances\. This provider is particularly useful for testing recipes on Amazon Linux, which is available only on Amazon EC2 instances\. The `vagrant-aws` provider is free, but you must have an AWS account and pay for any AWS resources that you use\.

At this point, you should go through Vagrant's [Getting Started walkthrough](http://docs.vagrantup.com/v2/getting-started/index.html), which describes how to install Vagrant on your workstation and teaches you the basics of how to use Vagrant\. Note that the examples in this chapter do not use a Git repository, so you can omit that part of the walkthrough if you prefer\.

### Test Kitchen<a name="cookbooks-101-tools-test-kitchen"></a>

[Test Kitchen](http://kitchen.ci/) simplifies the process of executing and testing your cookbooks on Vagrant\. As a practical matter, you rarely if ever need to use Vagrant directly\. Test Kitchen performs most common tasks, including:
+ Launching an instance in Vagrant\.
+ Transferring cookbooks to the instance\.
+ Running the cookbook's recipes on the instance\.
+ Testing a cookbook's recipes on the instance\.
+ Using SSH to log in to the instance\.

Instead of installing the Test Kitchen gem directly, we recommend installing [Chef DK](https://downloads.chef.io/chef-dk/)\. In addition to Chef itself, this package includes Test Kitchen, [Berkshelf](http://berkshelf.com/), [ChefSpec](https://docs.chef.io/chefspec.html), and several other useful tools\.

At this point, you should go through Test Kitchen's [Getting Started walkthrough](http://kitchen.ci/), which teaches you the basics of how to use Test Kitchen to execute and test recipes\. 

**Note**  
The examples in this chapter use Test Kitchen as a convenient way to run recipes\. If you prefer, you can stop the Getting Started walkthrough after completing the Manually Verifying section, which covers everything you need to know for the examples\. However, Test Kitchen is primarily a testing platform that supports test frameworks such as [bash automated test system \(BATS\)](https://github.com/sstephenson/bats)\. You should complete the remainder of the walkthrough at some point to learn how to use Test Kitchen to test your recipes\.