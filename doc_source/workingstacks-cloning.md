# Clone a Stack<a name="workingstacks-cloning"></a>

It is sometimes useful to create multiple copies of a stack\. For example, you might want to add redundancy as a disaster recovery or prevention measure, or you might use an existing stack as a starting point for a new stack\. The simplest approach is to clone the source stack\. On the AWS OpsWorks Stacks dashboard, in the **Actions** column of the row for the stack that you want to clone, choose **clone**, which opens the **Clone stack** page\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/clone_stack_action.png)

Initially, the settings for the cloned stack are identical to those for the source stack except that the word "copy" is appended to the stack name\. For information about these settings, see [Create a New Stack](workingstacks-creating.md)\. There are also two additional, optional settings: 

**Permissions**  
If **all permissions** is selected \(the default\), the source stack permissions are applied to the cloned stack\.

**Apps**  
Lists apps that are deployed on the source stack\. For each app listed, if the corresponding check box is selected \(the default\), the app is deployed to the cloned stack\. 

**Note**  
You cannot clone a stack from one regional endpoint to another; for example, you cannot clone a stack from the US West \(Oregon\) Region \(us\-west\-2\) to the Asia Pacific \(Mumbai\) Region \(ap\-south\-1\)\.

When you have finalized the settings, choose **Clone stack**\. AWS OpsWorks Stacks creates a new stack that consists of the source stack's layers and optionally its apps and permissions\. The layers have the same configuration as the originals, subject to any modifications that you made\. However, cloning does not create any instances\. You must add an appropriate set of instances to each layer of the cloned stack and then start them\. As with any stack, you can perform normal management tasks on a cloned stack, such as adding, deleting, or modifying layers or adding and deploying apps\.

To make the cloned stack operational, start the instances\. AWS OpsWorks Stacks sets up and configures each instance according to its layer membership\. It also deploys any applications, just as it does with a new stack\. 