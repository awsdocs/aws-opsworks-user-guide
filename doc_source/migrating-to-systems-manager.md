# Migrating your AWS OpsWorks Stacks applications to AWS Systems Manager Application Manager<a name="migrating-to-systems-manager"></a>

You can now migrate your AWS OpsWorks Stacks applications to [Application Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/application-manager.html), a capability of AWS Systems Manager, using a migration script\. Migrating your Stacks applications to Systems Manager Application Manager allows you to use AWS features that are not available in AWS OpsWorks Stacks, such as new Amazon EC2 instance types like Graviton, new Amazon Elastic Block Store \(EBS\) volumes like gp3, new operating systems, integrations with Auto Scaling groups, and application load balancers\.

With this release, you can now monitor and run operations on your migrated instances using a new **Instances** tab on the Systems Manager Application Manager page\. You can use the **Instances** tab to view multiple AWS instances in one place\. Using this tab, you can view information about instance health and troubleshoot issues\. For more information about working with the **Instances** tab, see [Working with your application instances](https://docs.aws.amazon.com/systems-manager/latest/userguide/application-manager-working-instances.html) in the *AWS Systems Manager User Guide*\.

**Topics**
+ [How the script works](#migrating-to-systems-manager-script)
+ [Prerequisites](migrating-to-systems-manager-prerequisites.md)
+ [Limitations](migrating-to-systems-manager-limitations.md)
+ [Getting started](migrating-to-systems-manager-getting-started.md)
+ [FAQ](migrating-to-systems-manager-faqs.md)
+ [Troubleshooting](migrating-to-systems-manager-troubleshooting.md)

## How the script works<a name="migrating-to-systems-manager-script"></a>

AWS OpsWorks provides a script that you can run to migrate your AWS OpsWorks Stacks applications to Systems Manager Application Manager using a CloudFormation template\. The script gets information about an existing OpsWorks layer and depending on the value of the `--provision-application` parameter for the script, either provisions a clone of your application, or provides a starter CloudFormation template that you can modify using AWS CloudFormation\. 