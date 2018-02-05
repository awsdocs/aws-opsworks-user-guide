# Step 5: Delete MyStack<a name="gettingstarted-delete"></a>

As soon as you begin using AWS resources like Amazon EC2 instances you are charged based on your usage\. If you are finished for now, you should stop the instances so that you do not incur any unwanted charges\. If you don’t need the stack anymore, you can delete it\.

**To delete MyStack**

1. 

**Stop all Instances**

   On the **Instances** page, click **Stop All Instances** and click **Stop** when asked confirm the operation\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gse1.png)

   After you click **Stop**, AWS OpsWorks Stacks terminates the associated Amazon EC2 instances, but not any associated resources such as Elastic IP addresses or Amazon EBS volumes\.

1. 

**Delete all Instances**

   Stopping the instance just terminates the associated Amazon EC2 instances\. After the instances status is in the stopped stated, you must delete each instance\. In the **PHP App Server** layer click **delete** in the php\-app1 instance's **Actions** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gse2.png)

   AWS OpsWorks Stacks then asks you to confirm the deletion, and shows you any dependent resources\. You can choose to keep any or all of these resources\. This example has no dependent resources, so just click **Delete**\.

   Repeat the process for php\-app2 and the **MySQL** instance, db\-master1\. Notice that db\-master1 has an associated Amazon Elastic Block Store volume, which is selected by default\. Leave it selected to delete the volume along with the instance\.

1. 

**Delete the Layers\.**

   On the **Layers** page, click **Delete** and then click **Delete** to confirm\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gse4.png)

   Repeat the process for the **MySQL** layer\.

1. 

**Delete the App**

   On the **Apps** page, click **delete** in the **SimplePHPApp** app's **Actions** column, and then click **Delete** to confirm\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gse5.png)

1. 

**Delete MyStack**

   On the **Stack** page, click **Delete Stack** and then click **Delete** to confirm\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gse6.png)

You have now reached the end of this walkthrough\.