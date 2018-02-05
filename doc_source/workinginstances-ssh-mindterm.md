# Using the Built\-in MindTerm SSH Client<a name="workinginstances-ssh-mindterm"></a>

The simplest way to log into a Linux instance is to use the built\-in MindTerm SSH client\. Each online instance includes an **ssh** action that you can use to launch the MindTerm client\.

**Note**  
You must have Java enabled in your browser to use the MindTerm client\.

**To log in with the MindTerm client**

1. If you haven't done so already, authorize SSH access for the IAM user that will be connecting to the instance, as described in the preceding section\. 

1. Log in as the IAM user\.

1. On the **Instances** page, choose **ssh** in the **Actions** column for the appropriate instance\.  
![\[ssh action on Instances page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/ssh.png)

1. For **Private key**, provide a path to the IAM user's personal private key or an Amazon EC2 private key, depending on which public keys you have installed on the instance\.

1. Choose **Launch Mindterm** and use the terminal window to run commands on the instance\.