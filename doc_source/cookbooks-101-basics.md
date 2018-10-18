# Cookbook Basics<a name="cookbooks-101-basics"></a>

You can use cookbooks to accomplish a wide variety of tasks\. The following topics assume that you are new to Chef, and describe how to use cookbooks to accomplish some common tasks\. Because Test Kitchen does not yet support Windows, the examples are all for Linux, with notes indicating how to adapt them for Windows\. If you are new to Chef, we recommend going through these examples, even if you will be working with Windows\. Most of the examples in this topic can be used on Windows instances with some modest changes, which are noted in the examples\. All of the examples run in a virtual machine, so you don't even need to have a Linux computer\. Just install Vagrant and Test Kitchen on your regular workstation\.

**Note**  
If you want to run these recipes on a Windows instance, the simplest approach is to create a Windows stack and run the recipes on one of the stack's instances\. For more information on how to run recipes on an AWS OpsWorks Stacks Windows instance, see [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md)\.

Before continuing, make sure that you have installed Vagrant and Test Kitchen, and gone through their Getting Started walkthroughs\. For more information, see [Vagrant and Test Kitchen](cookbooks-101.md#cookbooks-101-tools)\.

**Topics**
+ [Recipe Structure](cookbooks-101-basics-structure.md)
+ [Example 1: Installing Packages](cookbooks-101-basics-packages.md)
+ [Example 2: Managing Users](cookbooks-101-basics-users.md)
+ [Example 3: Creating Directories](cookbooks-101-basics-directories.md)
+ [Example 4: Adding Flow Control](cookbooks-101-basics-ruby.md)
+ [Example 5: Using Attributes](cookbooks-101-basics-attributes.md)
+ [Example 6: Creating Files](cookbooks-101-basics-files.md)
+ [Example 7: Running Commands and Scripts](cookbooks-101-basics-commands.md)
+ [Example 8: Managing Services](cookbooks-101-basics-services.md)
+ [Example 9: Using Amazon EC2 Instances](cookbooks-101-basics-ec2.md)
+ [Next Steps](cookbooks-101-basics-next.md)