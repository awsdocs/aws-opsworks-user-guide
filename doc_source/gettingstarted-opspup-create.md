# Create a Puppet Enterprise Master<a name="gettingstarted-opspup-create"></a>

You can create a Puppet master by using the AWS OpsWorks for Puppet Enterprise console, or the AWS CLI\. This walkthrough describes how to create a Puppet master in the AWS OpsWorks for Puppet Enterprise console\.

1. Sign in to the AWS Management Console and open the AWS OpsWorks console at [https://console\.aws\.amazon\.com/opsworks/](https://console.aws.amazon.com/opsworks/)\.

1. On the AWS OpsWorks home page, choose **Go to OpsWorks for Puppet Enterprise**\.  
![\[AWS OpsWorks service home\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_day0.png)

1. On the AWS OpsWorks for Puppet Enterprise home page, choose **Create Puppet Enterprise server**\.  
![\[Puppet master dashboard\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_dashboardhome.png)

1. On the **Set name, region, and type** page, specify a name for your server\. Puppet master names can be a maximum of 40 characters, must start with a letter, and can contain only alphanumeric characters and dashes\. Select a supported region, and then choose an instance type that supports the number of nodes that you want to manage\. You can change the instance type after your server has been created, if needed\. For this walkthrough, we are creating a **c4\.large** instance type in the US West \(Oregon\) Region\. Choose **Next**\.  
![\[Set name, region, and type page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_setname.png)

1. On the **Configure credentials** page, leave the default selection in the **SSH key** drop\-down list, unless you want to specify a key pair name\. In the **r10k remote** field of the **Configure Puppet Code Manager** area, specify a valid SSH URL of your Git remote\. In the **r10k private key** field, paste in the SSH private key that AWS OpsWorks can use to access the r10k remote repository\. This is provided by Git when you create a private repository\. Choose **Next**\.  
![\[Configure credentials page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_configcreds.png)

1. On the **Configure advanced settings** page, in the **Network and security** area, choose a VPC, subnet, and one or more security groups\. AWS OpsWorks can generate a security group, service role, and instance profile for you, if you do not already have ones that you want to use\. Your server can be a member of multiple security groups\. You cannot change network and security settings for the Puppet master after you have left this page\.  
![\[Network and security\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_network_sec.png)

1. In the **System maintenance** section, set the day and hour that you want system maintenance to begin\. Because you should expect the server to be offline during system maintenance, choose a time of low server demand within regular office hours\.

   The maintenance window is required\. You can change the start day and time later by using the AWS Management Console, AWS CLI, or the APIs\.  
![\[System maintenance\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_sysmaint.png)

1. Configure backups\. By default, automatic backups are enabled\. Set a preferred frequency and hour for automatic backup to start, and set the number of backup generations to store in Amazon Simple Storage Service\. A maximum of 30 backups can be kept; when the maximum is reached, AWS OpsWorks for Puppet Enterprise deletes the oldest backups to make room for new ones\.  
![\[Automatic backups\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_backupconfig.png)

1. When you are finished configuring advanced settings, choose **Next**\.

1. On the **Review** page, review your choices\. When you are ready to create the server, choose **Launch**\.

   While you are waiting for AWS OpsWorks to create your Puppet master, go on to [Configure the Puppet Master Using the Starter Kit](opspup-starterkit.md) and download the Starter Kit and the Puppet Enterprise console credentials\. Do not wait until your server is online to download these items\. 

   When server creation is finished, your Puppet master is available on the AWS OpsWorks for Puppet Enterprise home page, with a status of **online**\. After the server is online, the Puppet Enterprise console is available on the server's domain, at a URL in the following format: `https://your_server_name-randomID.region.opsworks-cm.io`\.