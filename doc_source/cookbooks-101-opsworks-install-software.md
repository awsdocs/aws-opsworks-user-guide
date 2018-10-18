# Installing Windows Software<a name="cookbooks-101-opsworks-install-software"></a>

**Note**  
These examples assume that you have already done the [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md) example\. If not, you should do that example first\. In particular, it describes how to enable RDP access to your instances\.

 Windows instances start with Windows Server 2012 R2 Standard, so you typically need to install some software\. The details depend on the type of software\.
+  Windows features are optional system components, including the \.NET frameworks and Internet Information Server \(IIS\), which you can download to your instance\.
+ Third\-party software typically comes in an installer package, such as an MSI file, which you must download to the instance and then run\.

  Some Microsoft software also comes in an installer package\.

This section describes how to implement cookbooks to install Windows features and packages\. It also introduces the Chef windows cookbook, which contains resources and helper functions that simplify implementing recipes for Windows instances\.

**Topics**
+ [Installing a Windows Feature: IIS](cookbooks-101-opsworks-install-software-feature.md)
+ [Installing a Package on a Windows Instance](cookbooks-101-opsworks-install-software-package.md)