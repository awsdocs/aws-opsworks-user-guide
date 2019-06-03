# Create a Chef Automate Server<a name="gettingstarted-opscm-create"></a>

You can create a Chef server by using the AWS OpsWorks for Chef Automate console, or the AWS CLI\.

**Topics**
+ [Create a Chef Automate server in the AWS Management Console](#gettingstarted-opscm-create-console)
+ [Create a Chef Automate server by using the AWS CLI](#gettingstarted-opscm-create-cli)

## Create a Chef Automate server in the AWS Management Console<a name="gettingstarted-opscm-create-console"></a>

1. Sign in to the AWS Management Console and open the AWS OpsWorks console at [https://console\.aws\.amazon\.com/opsworks/](https://console.aws.amazon.com/opsworks/)\.

1. On the AWS OpsWorks home page, choose **Go to OpsWorks for Chef Automate**\.  
![\[AWS OpsWorks service home\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_day0.png)

1. On the AWS OpsWorks for Chef Automate home page, choose **Create Chef Automate server**\.  
![\[Chef Automate servers home\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_dashboardhome.png)

1. On the **Set name, region, and type** page, specify a name for your server\. Chef server names can be a maximum of 40 characters, and can contain only alphanumeric characters and dashes\. Select a supported region, and then choose an instance type that supports the number of nodes that you want to manage\. You can change the instance type after your server has been created, if needed\. For this walkthrough, we are creating an **m5\.large** instance type in the US West \(Oregon\) Region\. Choose **Next**\.  
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

## Create a Chef Automate server by using the AWS CLI<a name="gettingstarted-opscm-create-cli"></a>

Creating an AWS OpsWorks for Chef Automate server by running AWS CLI commands differs from creating a server in the console\. In the console, AWS OpsWorks creates a service role and security group for you, if you do not specify existing ones that you want to use\. In the AWS CLI, AWS OpsWorks can create a security group for you if you do not specify one, but it does not automatically create a service role; you must provide a service role ARN as part of your `create-server` command\. In the console, while AWS OpsWorks is creating your Chef Automate server, you download the Chef Automate starter kit and the sign\-in credentials for the Chef Automate dashboard\. Because you cannot do this when you create an AWS OpsWorks for Chef Automate server by using the AWS CLI, you use a JSON processing utility to get the sign\-in credentials and the starter kit from the results of the `create-server` command after your new AWS OpsWorks for Chef Automate server is online\. Alternatively, you can generate a new set of sign\-in credentials and a new starter kit in the console after your new AWS OpsWorks for Chef Automate server is online\.

If your local computer is not already running the AWS CLI, download and install the AWS CLI by following [installation instructions](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\. This section does not describe all parameters that you can use with the `create-server` command\. For more information about `create-server` parameters, see [https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/create-server.html](https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/create-server.html) in the *AWS CLI Reference*\.

1. Be sure to complete the prerequisites, especially [Set Up a VPC](gettingstarted-opscm.md#set-up-vpc), or be sure that you have an existing VPC that you want to use\. To create your Chef Automate server, you need a subnet ID\.

1. Optionally, generate a Chef pivotal key by using [OpenSSL](https://www.openssl.org/), and save the key to a secure, convenient file on your local computer\. The pivotal key is automatically generated as part of the server creation process if you do not provide one in the `create-server` command\. If you want to skip this step, you can instead get the Chef Automate pivotal key from the results of the `create-server` command\. If you choose to generate the pivotal key using the following commands, be sure to include the `-pubout` parameter, because the Chef Automate pivotal key value is the public half of the RSA key pair\. For more information, see Step 6\.

   ```
   umask 077
   openssl genrsa -out "pivotal" 2048
   openssl rsa -in "pivotal" -pubout
   ```

1. Create a service role and an instance profile\. AWS OpsWorks provides an AWS CloudFormation template that you can use to create both\. Run the following AWS CLI command to create an AWS CloudFormation stack that creates the service role and instance profile for you\.

   ```
   aws cloudformation create-stack --stack-name OpsWorksCMRoles --template-url https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-roles.yaml --capabilities CAPABILITY_NAMED_IAM
   ```

1. After AWS CloudFormation finishes creating the stack, find and copy the ARNs of service roles in your account\.

   ```
   aws iam list-roles --path-prefix "/service-role/" --no-paginate
   ```

   In the results of the `list-roles` command, look for service role ARN entries that resemble the following\. Make a note of the service role ARNs\. You need these values to create your Chef Automate server\.

   ```
   {
       "AssumeRolePolicyDocument": {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Action": "sts:AssumeRole",
                   "Effect": "Allow",
                   "Principal": {
                       "Service": "ec2.amazonaws.com"
                   }
               }
           ]
       },
       "RoleId": "AROZZZZZZZZZZQG6R22HC",
       "CreateDate": "2018-01-05T20:42:20Z",
       "RoleName": "aws-opsworks-cm-ec2-role",
       "Path": "/service-role/",
       "Arn": "arn:aws:iam::000000000000:role/service-role/aws-opsworks-cm-ec2-role"
   },
   {
       "AssumeRolePolicyDocument": {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Action": "sts:AssumeRole",
                   "Effect": "Allow",
                   "Principal": {
                       "Service": "opsworks-cm.amazonaws.com"
                   }
               }
           ]
       },
       "RoleId": "AROZZZZZZZZZZZZZZZ6QE",
       "CreateDate": "2018-01-05T20:42:20Z",
       "RoleName": "aws-opsworks-cm-service-role",
       "Path": "/service-role/",
       "Arn": "arn:aws:iam::000000000000:role/service-role/aws-opsworks-cm-service-role"
   }
   ```

1. Find and copy the ARNs of instance profiles in your account\.

   ```
   aws iam list-instance-profiles --no-paginate
   ```

   In the results of the `list-instance-profiles` command, look for instance profile ARN entries that resemble the following\. Make a note of the instance profile ARNs\. You need these values to create your Chef Automate server\.

   ```
   {
       "Path": "/",
       "InstanceProfileName": "aws-opsworks-cm-ec2-role",
       "InstanceProfileId": "EXAMPLEDC6UR3LTUW7VHK",
       "Arn": "arn:aws:iam::123456789012:instance-profile/aws-opsworks-cm-ec2-role",
       "CreateDate": "2017-01-05T20:42:20Z",
       "Roles": [
           {
               "Path": "/service-role/",
               "RoleName": "aws-opsworks-cm-ec2-role",
               "RoleId": "EXAMPLEE4STNUQG6R22HC",
               "Arn": "arn:aws:iam::123456789012:role/service-role/aws-opsworks-cm-ec2-role",
               "CreateDate": "2017-01-05T20:42:20Z",
               "AssumeRolePolicyDocument": {
                   "Version": "2012-10-17",
                   "Statement": [
                       {
                           "Effect": "Allow",
                           "Principal": {
                               "Service": "ec2.amazonaws.com"
                           },
                           "Action": "sts:AssumeRole"
                       }
                   ]
               }
           }
       ]
   },
   ```

1. Create the AWS OpsWorks for Chef Automate server by running the `create-server` command\.
   + The `--engine` value is `ChefAutomate`, `--engine-model` is `Single`, and `--engine-version` is `12`\.
   + The server name must be unique within your AWS account, within each region\. Server names must start with a letter; then letters, numbers, or hyphens \(\-\) are allowed, up to a maximum of 40 characters\.
   + Use the instance profile ARN and service role ARN that you copied in Steps 4 and 5\.
   + Valid instance types are `m5.large`, `r5.xlarge`, or `r5.2xlarge`\. For more information about the specifications of these instance types, see [Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon EC2 User Guide*\.
   + The `--engine-attributes` parameter is optional; if you don't specify one or both values, the server creation process generates the values for you\. If you add `--engine-attributes`, specify either the `CHEF_AUTOMATE_PIVOTAL_KEY` value that you generated in Step 2, a `CHEF_AUTOMATE_ADMIN_PASSWORD`, or both\.

     If you do not set a value for `CHEF_AUTOMATE_ADMIN_PASSWORD`, a password is generated and returned as part of the `create-server` response\. You can also download the starter kit again in the console, which regenerates this password\. The password length is a minimum of eight characters, and a maximum of 32\. The password can contain letters, numbers, and special characters \(`!/@#$%^+=_`\)\. The password must contain at least one lower case letter, one upper case letter, one number, and one special character\. 
   + An SSH key pair is optional, but can help you connect to your Chef Automate server if you need to reset the Chef Automate dashboard administrator password\. For more information about creating an SSH key pair, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide*\.
   + Weekly system maintenance is required\. Valid values must be specified in the following format: `DDD:HH:MM`\. The specified time is in coordinated universal time \(UTC\)\. If you do not specify a value for `--preferred-maintenance-window`, the default value is a random, one\-hour period on Tuesday, Wednesday, or Friday\.
   + Valid values for `--preferred-backup-window` must be specified in one of the following formats: `HH:MM` for daily backups, or `DDD:HH:MM` for weekly backups\. The specified time is in UTC\. The default value is a random, daily start time\. To opt out of automatic backups, add the parameter `--disable-automated-backup` instead\.
   + For `--security-group-ids`, enter one or more security group IDs, separated by a space\.
   + For `--subnet-ids`, enter a subnet ID\.

   ```
   aws opsworks-cm create-server --engine "ChefAutomate" --engine-model "Single" --engine-version "12" --server-name "server_name" --instance-profile-arn "instance_profile_ARN" --instance-type "instance_type" --engine-attributes '{"CHEF_AUTOMATE_PIVOTAL_KEY":"pivotal_key","CHEF_AUTOMATE_ADMIN_PASSWORD":"password"}' --key-pair "key_pair_name" --preferred-maintenance-window "ddd:hh:mm" --preferred-backup-window "ddd:hh:mm" --security-group-ids security_group_id1 security_group_id2 --service-role-arn "service_role_ARN" --subnet-ids subnet_ID
   ```

   The following is an example\.

   ```
   aws opsworks-cm create-server --engine "ChefAutomate" --engine-model "Single" --engine-version "12" --server-name "automate-06" --instance-profile-arn "arn:aws:iam::12345678912:instance-profile/aws-opsworks-cm-ec2-role" --instance-type "m5.large" --engine-attributes '{"CHEF_AUTOMATE_PIVOTAL_KEY":"MZZE...Wobg","CHEF_AUTOMATE_ADMIN_PASSWORD":"zZZzDj2DLYXSZFRv1d"}' --key-pair "amazon-test" --preferred-maintenance-window "Mon:08:00" --preferred-backup-window "Sun:02:00" --security-group-ids sg-b00000001 sg-b0000008 --service-role-arn "arn:aws:iam::12345678912:role/service-role/aws-opsworks-cm-service-role" --subnet-ids subnet-300aaa00
   ```

1. AWS OpsWorks for Chef Automate takes about 15 minutes to create a new server\. Do not dismiss the output of the `create-server` command or close your shell session, because the output can contain important information that is not shown again\. To get passwords and the starter kit from the `create-server` results, go on to the next step\.

1. If you opted to have AWS OpsWorks for Chef Automate generate a key and password for you, you can extract them in usable formats from the `create-server` results by using a JSON processor such as [jq](https://stedolan.github.io/jq/)\. After you install [jq](https://stedolan.github.io/jq/), you can run the following commands to extract the pivotal key, Chef Automate dashboard administrator password, and starter kit\. If you did not provide your own pivotal key and password in Step 4, be sure to save the extracted pivotal key and administrator password in convenient but secure locations\.

   ```
   #Get the Chef password:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "CHEF_AUTOMATE_ADMIN_PASSWORD") | .Value'
   
   #Get the Chef Pivotal Key:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "CHEF_AUTOMATE_PIVOTAL_KEY") | .Value'
   
   #Get the Chef Starter Kit:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "CHEF_STARTER_KIT") | .Value' | base64 -D > starterkit.zip
   ```

1. Optionally, if you did not extract the starter kit from `create-server` command results, you can download a new starter kit from the server's Properties page in the AWS OpsWorks for Chef Automate console\. Downloading a new starter kit resets the Chef Automate dashboard administrator password\.

1. When the server creation process is finished, go on to [Configure the Chef Server Using the Starter Kit](opscm-starterkit.md)\.