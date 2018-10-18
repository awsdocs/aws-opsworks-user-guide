# Implementing Cookbooks for AWS OpsWorks Stacks<a name="cookbooks-101-opsworks"></a>

[Cookbook Basics](cookbooks-101-basics.md) introduced you to cookbooks and recipes\. The examples in that section were simple by design and will work on any instance that supports Chef, including AWS OpsWorks Stacks instances\. To implement more sophisticated cookbooks for AWS OpsWorks Stacks, you typically need to take full advantage of the AWS OpsWorks Stacks environment, which differs from standard Chef in a number of ways\.

This topic describes the basics of implementing recipes for AWS OpsWorks Stacks instances\. 

**Note**  
If you are not familiar with how to implement cookbooks, you should start with [Cookbook Basics](cookbooks-101-basics.md)\.

**Topics**
+ [Running a Recipe on an AWS OpsWorks Stacks Linux Instance](cookbooks-101-opsworks-opsworks-instance.md)
+ [Running a Recipe on a Windows Instance](cookbooks-101-opsworks-opsworks-windows.md)
+ [Running a Windows PowerShell Script](cookbooks-101-opsworks-opsworks-powershell.md)
+ [Mocking the Stack Configuration and Deployment Attributes on Vagrant](opsworks-opsworks-mock.md)
+ [Using Stack Configuration and Deployment Attribute Values](cookbooks-101-opsworks-opsworks-stack-config.md)
+ [Using an External Cookbook on a Linux Instance: Berkshelf](cookbooks-101-opsworks-berkshelf.md)
+ [Using the SDK for Ruby: Downloading Files from Amazon S3](cookbooks-101-opsworks-s3.md)
+ [Installing Windows Software](cookbooks-101-opsworks-install-software.md)
+ [Overriding Built\-In Attributes](cookbooks-101-opsworks-attributes.md)
+ [Overriding Built\-In Templates](cookbooks-101-opsworks-templates.md)