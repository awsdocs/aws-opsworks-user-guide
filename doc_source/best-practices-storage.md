# Best Practices: Root Device Storage for Instances<a name="best-practices-storage"></a>

**Note**  
This topic does not apply to Windows instances, which must be Amazon Elastic Block Store\-backed\.

Amazon Elastic Compute Cloud \(Amazon EC2\) Linux instances have the following root\-device storage options\.
+ **Instance store\-backed instances** – The root device is temporary\.

  If you stop the instance, the data on the root device vanishes and cannot be recovered\. For more information, see [Amazon EC2 Instance Store](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)\.
+ **Amazon EBS\-backed instances** – The root device is an Amazon EBS volume\.

  If you stop the instance, the Amazon EBS volume persists\. If you restart the instance, the volume is automatically remounted, restoring the instance state and any stored data\. You can also mount the volume on a different instance\. For more information, see [Amazon Elastic Block Store \(Amazon EBS\)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)\.

Consider the following when deciding which root device storage option to use\.

**Boot Time**  
After the initial start, Amazon EBS instances generally restart faster\.  
The initial startup time is approximately the same for either storage type\. Both types must perform a full setup, which includes relatively time\-consuming tasks such as installing packages from remote repositories\. However, note these distinctions when you subsequently restart an instance:  
+ Instance store\-backed instances perform the same setup tasks that they did for the initial start, including package installation\.

  A restart takes about the same time as the initial start\.
+ Amazon EBS\-back instances remount the root volume and run the Setup recipes\.

  The restart is usually significantly faster than the initial start, because the Setup recipes don't have to perform tasks such as reinstalling packages that are already installed on the root volume\.

**Cost**  
Amazon EBS\-backed instances are more costly:  
+ With an instance\-store backed instance, you pay only when the instance is running\.
+ With Amazon EBS\-backed instances, you pay for the Amazon EBS volume whether the instance is running or not\.

  For more information, see [Amazon EBS Pricing](http://aws.amazon.com/ebs/pricing/)\.

**Logging**  
Amazon EBS\-backed instances automatically retain logs:  
+ With instance store\-backed instance, the logs disappear when the instance stops\.

  You must either retrieve the logs before you stop the instance or use a service such as [CloudWatch Logs](monitoring-cloudwatch-logs.md) to store selected logs remotely\.
+ With an Amazon EBS\-backed instance, the logs are stored on the Amazon EBS volume\.

  You can view them by restarting the instance, or by mounting the volume on another instance\. 

**Dependencies**  
The two storage types have different dependencies:  
+ Instance\-store backed instances depend on Amazon S3\.

  When you start the instance, it must download the AMI from Amazon S3\.
+ Amazon EBS\-backed instances depend on Amazon EBS\.

  When you start the instance, it must mount the Amazon EBS root volume\.

**Recommendation:** If you aren't certain which storage type is best suited for your requirements, we recommend starting with Amazon EBS instances\. Although you will incur a modest expense for the Amazon EBS volumes, there is less risk of unintended data loss\.