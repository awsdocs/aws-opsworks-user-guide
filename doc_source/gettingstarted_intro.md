# Getting Started with AWS OpsWorks Stacks<a name="gettingstarted_intro"></a>

AWS OpsWorks Stacks provides a rich set of customizable components that you can mix and match to create a stack that satisfies your specific purposes\. The challenge for new users is understanding how to assemble these components into a working stack and manage it effectively\. Here's how you can get started\.


| If you want to\.\.\. | Complete this walkthrough: | 
| --- | --- | 
| Create a sample stack as quick as possible | [Getting Started: Sample](gettingstarted-intro.md)  | 
| Experiment with a Linux\-based stack | [Getting Started: Linux](gettingstarted-linux.md) | 
| Experiment with a Windows\-based stack | [Getting Started: Windows](gettingstarted-windows.md) | 
| Learn how to create your own Chef cookbooks | [Getting Started: Cookbooks](gettingstarted-cookbooks.md) | 

If you have existing computing resources—Amazon EC2 instances or even *on\-premises* instances that are running on your own hardware—you can [incorporate them into a stack](registered-instances.md), along with instances that you created with AWS OpsWorks Stacks\. You can then use AWS OpsWorks Stacks to manage all related instance as a group, regardless of how they were created\.

## Region Support<a name="gettingstarted-intro-region"></a>

You can access AWS OpsWorks Stacks globally; you can also create and manage instances globally\. Users can configure AWS OpsWorks Stacks instances to be launched in any AWS region except AWS GovCloud \(US\-West\) and the China \(Beijing\) Region\. To work with AWS OpsWorks Stacks, instances must be able to connect to one of the following AWS OpsWorks Stacks instance service API endpoints\.

Resources can be managed only in the region in which they are created\. Resources that are created in one regional endpoint are not available, nor can they be cloned to, another regional endpoint\. You can launch instances in any of the following regions\.
+ US East \(Ohio\) Region
+ US East \(N\. Virginia\) Region
+ US West \(Oregon\) Region
+ US West \(N\. California\) Region
+ Canada \(Central\) Region \(API only, not available for stacks created in the AWS Management Console\.\)
+ Asia Pacific \(Mumbai\) Region
+ Asia Pacific \(Singapore\) Region
+ Asia Pacific \(Sydney\) Region
+ Asia Pacific \(Tokyo\) Region
+ Asia Pacific \(Seoul\) Region
+ EU \(Frankfurt\) Region
+ EU \(Ireland\) Region
+ EU \(London\) Region
+ EU \(Paris\) Region
+ South America \(São Paulo\) Region