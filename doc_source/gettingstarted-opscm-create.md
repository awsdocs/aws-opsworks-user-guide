# Create a Chef Automate Server<a name="gettingstarted-opscm-create"></a>

You can create a Chef server by using the AWS OpsWorks for Chef Automate console, or the AWS CLI\. This walkthrough describes how to create a Chef server in the AWS OpsWorks for Chef Automate console\.

1. Sign in to the AWS Management Console and open the AWS OpsWorks console at [https://console\.aws\.amazon\.com/opsworks/](https://console.aws.amazon.com/opsworks/)\.

1. On the AWS OpsWorks Stacks home page, choose **Go to OpsWorks for Chef Automate**\.  
![\[AWS OpsWorks service home\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_day0.png)

1. On the AWS OpsWorks for Chef Automate home page, choose **Create Chef Automate server**\.  
![\[Chef Automate servers dashboard\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_dashboardhome.png)

1. On the **Set name, region, and type** page, specify a name for your server\. Chef server names can be a maximum of 40 characters, and can contain only alphanumeric characters and dashes\. Select a supported region, and then choose an instance type that supports the number of nodes that you want to manage\. You can change the instance type after your server has been created, if needed\. For this walkthrough, we are creating a **t2\.medium** instance type in the US West \(Oregon\) Region\. Choose **Next**\.  
![\[Set name, region, and type page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_setname.png)

1. On the **Select an SSH key** page, leave the default selection in the **SSH key** drop\-down list, unless you want to specify a key pair name\. Choose **Next**\.  
![\[Select an SSH key page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_keypair.png)

1. On the **Configure Advanced Settings** page, in the **Network and Security** area, choose a VPC, subnet, and one or more security groups\. AWS OpsWorks can generate a security group, service role, and instance profile for you, if you do not already have ones that you want to use\. Your server can be a member of multiple security groups\. You cannot change network and security settings for the Chef server after you have left this page\.  
![\[Network and security\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_networksec.png)

1. In the **System maintenance** section, set the day and hour that you want system maintenance to begin\. Because you should expect the server to be offline during system maintenance, choose a time of low server demand within regular office hours\. Connected nodes enter a `pending-server` state until maintenance is complete\.

   The maintenance window is required\. You can change the start day and time later by using the AWS Management Console, AWS CLI, or the APIs\.  
![\[System maintenance\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_sysmaint.png)

1. Configure backups\. By default, automatic backups are enabled\. Set a preferred frequency and hour for automatic backup to start, and set the number of backup generations to store in Amazon Simple Storage Service\. A maximum of 30 backups are kept; when the maximum is reached, AWS OpsWorks for Chef Automate deletes the oldest backups to make room for new ones\.  
![\[Automatic backups\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_backupconfig.png)

1. When you are finished configuring advanced settings, choose **Next**\.

1. On the **Review** page, review your choices\. When you are ready to create the server, choose **Launch**\.

   While you are waiting for AWS OpsWorks to create your Chef server, go on to [Configure the Chef Server Using the Starter Kit](opscm-starterkit.md) and download the Starter Kit and the Chef Automate dashboard credentials\. Do not wait until your server is online to download these items\. 

   When server creation is finished, your Chef server is available on the AWS OpsWorks for Chef Automate home page, with a status of **online**\. After the server is online, the Chef Automate dashboard is available on the server's domain, at a URL in the following format: `https://your_server_name-random.region.opsworks-cm.io`\.