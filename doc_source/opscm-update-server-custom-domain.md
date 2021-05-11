# Update an AWS OpsWorks for Chef Automate Server to Use a Custom Domain<a name="opscm-update-server-custom-domain"></a>

This section describes how to update an existing AWS OpsWorks for Chef Automate server to use a custom domain and certificate by using a backup of the server to create a new server\. Essentially, you are copying an existing AWS OpsWorks for Chef Automate 2\.0 server by creating a new server from a backup, then configuring the new server to use a custom domain, certificate, and private key\.

**Topics**
+ [Prerequisites](#opscm-update-server-custom-domain-reqs)
+ [Limitations](#opscm-update-server-custom-domain-limits)
+ [Update a Server to Use a Custom Domain](#opscm-update-server-custom-domain-howto)
+ [See Also](#opscm-update-server-custom-domain-seealso)

## Prerequisites<a name="opscm-update-server-custom-domain-reqs"></a>

The following are requirements for updating an existing AWS OpsWorks for Chef Automate server to use a custom domain and certificate\.
+ The server that you want to update \(or copy\) must be running Chef Automate 2\.0\.
+ Decide which backup you want to use to create a new server\. You must have at least one backup available of the server that you want to update\. For more information about backups in AWS OpsWorks for Chef Automate, see [Back Up an AWS OpsWorks for Chef Automate Server](opscm-chef-backup.md)\.
+ Have ready the service role and instance profile ARNs that you used to create the existing server that is the source of your backup\.
+ Be sure that you are running the most current release of the AWS CLI\. For more information about updating your AWS CLI tools, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) in the *AWS Command Line Interface User Guide*\.

## Limitations<a name="opscm-update-server-custom-domain-limits"></a>

When you update an existing server by creating a new server from a backup, the new server cannot be exactly the same as the existing AWS OpsWorks for Chef Automate server\.
+ You can only complete this procedure by using the AWS CLI or one of the [AWS SDKs](https://docs.aws.amazon.com/#sdks)\. You cannot create a new server from a backup by using the AWS Management Console\.
+ The new server cannot use the same name as the existing server within an account, and within an AWS Region\. The name must be different from the existing server that you used as the source of the backup\.
+ Nodes that were attached to the existing server are not managed by the new server\. You must do one of the following\.
  + Attach different nodes, because nodes cannot be managed by more than one Chef Automate server\.
  + Migrate the nodes from the existing server \(the source of the backup\) to the new server and the new custom domain endpoint\. For more information about how to migrate nodes, see in the Chef documentation\.

## Update a Server to Use a Custom Domain<a name="opscm-update-server-custom-domain-howto"></a>

To update an existing Chef Automate 2\.0 server, you make a copy of it by running the `create-server` command, adding parameters to specify a backup, a custom domain, a custom certificate, and a custom private key\.

1. If you do not have service role or instance profile ARNs available to specify in your `create-server` command, follow steps 1\-5 in [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli) to create a service role and instance profile that you can use\.

1. If you have not already done so, find the backup of the existing Chef Automate 2\.0 server on which you want to base a new server with a custom domain\. Run the following command to show information about all AWS OpsWorks for Chef Automate backups in your account, and in a region\. Be sure to note the ID of the backup that you want to use\.

   ```
   aws opsworks-cm --region region name describe-backups
   ```

1. Create the AWS OpsWorks for Chef Automate server by running the `create-server` command\.
   + The `--engine` value is `ChefAutomate`, `--engine-model` is `Single`, and `--engine-version` is `12`\.
   + The server name must be unique within your AWS account, within each region\. Server names must start with a letter; then letters, numbers, or hyphens \(\-\) are allowed, up to a maximum of 40 characters\.
   + Use the instance profile ARN and service role ARN from step 1\.
   + Valid instance types are `m5.large`, `r5.xlarge`, or `r5.2xlarge`\. For more information about the specifications of these instance types, see [Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon EC2 User Guide*\.
   + The `--engine-attributes` parameter is optional; if you don't specify one or both values, the server creation process generates the values for you\. If you add `--engine-attributes`, specify either the `CHEF_AUTOMATE_PIVOTAL_KEY` value that you generated in Step 2, a `CHEF_AUTOMATE_ADMIN_PASSWORD`, or both\.

     If you do not set a value for `CHEF_AUTOMATE_ADMIN_PASSWORD`, a password is generated and returned as part of the `create-server` response\. You can also download the starter kit again in the console, which regenerates this password\. The password length is a minimum of eight characters, and a maximum of 32\. The password can contain letters, numbers, and special characters \(`!/@#$%^+=_`\)\. The password must contain at least one lower case letter, one upper case letter, one number, and one special character\. 
   + An SSH key pair is optional, but can help you connect to your Chef Automate server if you need to reset the Chef Automate dashboard administrator password\. For more information about creating an SSH key pair, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide*\.
   + To use a custom domain, add the following parameters to your command\. Otherwise, the Chef Automate server creation process automatically generates an endpoint for you\. All three parameters are required to configure a custom domain\. For information about additional requirements for using these parameters, see [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) in the AWS OpsWorks CM API Reference\.
     + `--custom-domain` \- An optional public endpoint of a server, such as `https://aws.my-company.com`\.
     + `--custom-certificate` \- A PEM\-formatted HTTPS certificate\. The value can be be a single, self\-signed certificate, or a certificate chain\.
     + `--custom-private-key` \- A private key in PEM format for connecting to the server by using HTTPS\. The private key must not be encrypted; it cannot be protected by a password or passphrase\.
   + Weekly system maintenance is required\. Valid values must be specified in the following format: `DDD:HH:MM`\. The specified time is in coordinated universal time \(UTC\)\. If you do not specify a value for `--preferred-maintenance-window`, the default value is a random, one\-hour period on Tuesday, Wednesday, or Friday\.
   + Valid values for `--preferred-backup-window` must be specified in one of the following formats: `HH:MM` for daily backups, or `DDD:HH:MM` for weekly backups\. The specified time is in UTC\. The default value is a random, daily start time\. To opt out of automatic backups, add the parameter `--disable-automated-backup` instead\.
   + For `--security-group-ids`, enter one or more security group IDs, separated by a space\.
   + For `--subnet-ids`, enter a subnet ID\.
   + For `--backup-id`, enter the ID of the backup that you copied in step 2\.

   ```
   aws opsworks-cm create-server --engine "ChefAutomate" --engine-model "Single" --engine-version "12" --server-name "server_name" --instance-profile-arn "instance_profile_ARN" --instance-type "instance_type" --engine-attributes '{"CHEF_AUTOMATE_PIVOTAL_KEY":"pivotal_key","CHEF_AUTOMATE_ADMIN_PASSWORD":"password"}' --key-pair "key_pair_name" --preferred-maintenance-window "ddd:hh:mm" --preferred-backup-window "ddd:hh:mm" --security-group-ids security_group_id1 security_group_id2 --service-role-arn "service_role_ARN" --subnet-ids subnet_ID --backup-id backup_ID
   ```

   The following example creates a Chef Automate server that uses a custom domain\.

   ```
   aws opsworks-cm create-server --engine "ChefAutomate" --engine-model "Single" --engine-version "12" \
       --server-name "my-custom-domain-server" \
       --instance-profile-arn "arn:aws:iam::12345678912:instance-profile/aws-opsworks-cm-ec2-role" \
       --instance-type "m5.large" \
       --engine-attributes '{"CHEF_AUTOMATE_PIVOTAL_KEY":"MZZE...Wobg","CHEF_AUTOMATE_ADMIN_PASSWORD":"zZZzDj2DLYXSZFRv1d"}' \
       --custom-domain "my-chef-automate-server.my-corp.com" \
       --custom-certificate "-----BEGIN CERTIFICATE----- EXAMPLEqEXAMPLE== -----END CERTIFICATE-----" \
       --custom-private-key "-----BEGIN RSA PRIVATE KEY----- EXAMPLEqEXAMPLE= -----END RSA PRIVATE KEY-----" \
       --key-pair "amazon-test" \
       --preferred-maintenance-window "Mon:08:00" \
       --preferred-backup-window "Sun:02:00" \
       --security-group-ids sg-b00000001 sg-b0000008 \
       --service-role-arn "arn:aws:iam::12345678912:role/service-role/aws-opsworks-cm-service-role" \
       --subnet-ids subnet-300aaa00 \
       --backup-id MyChefServer-20191004122143125
   ```

1. AWS OpsWorks for Chef Automate takes about 15 minutes to create a new server\. In the output of the `create-server` command, copy the value of the `Endpoint` attribute\. The following is an example\.

   ```
   "Endpoint": "automate-07-exampleexample.opsworks-cm.us-east-1.amazonaws.com"
   ```

   Do not dismiss the output of the `create-server` command or close your shell session, because the output can contain important information that is not shown again\. To get passwords and the starter kit from the `create-server` results, go on to the next step\.

1. If you opted to have AWS OpsWorks for Chef Automate generate a key and password for you, you can extract them in usable formats from the `create-server` results by using a JSON processor such as [jq](https://stedolan.github.io/jq/)\. After you install [jq](https://stedolan.github.io/jq/), you can run the following commands to extract the pivotal key, Chef Automate dashboard administrator password, and starter kit\. If you did not provide your own pivotal key and password in Step 3, be sure to save the extracted pivotal key and administrator password in convenient but secure locations\.

   ```
   #Get the Chef password:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "CHEF_AUTOMATE_ADMIN_PASSWORD") | .Value'
   
   #Get the Chef Pivotal Key:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "CHEF_AUTOMATE_PIVOTAL_KEY") | .Value'
   
   #Get the Chef Starter Kit:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "CHEF_STARTER_KIT") | .Value' | base64 -D > starterkit.zip
   ```

1. Optionally, if you did not extract the starter kit from `create-server` command results, you can download a new starter kit from the server's Properties page in the AWS OpsWorks for Chef Automate console\. Downloading a new starter kit resets the Chef Automate dashboard administrator password\.

1. Create a CNAME entry in your enterprise's DNS management tool to point your custom domain to the AWS OpsWorks for Chef Automate endpoint that you copied in step 4\. You cannot reach or sign in to the server until you complete this step\.

1. When the server creation process is finished, go on to [Configure the Chef Server Using the Starter Kit](opscm-starterkit.md)\.

## See Also<a name="opscm-update-server-custom-domain-seealso"></a>
+ [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli)
+ [Restore an AWS OpsWorks for Chef Automate Server from a Backup](opscm-chef-restore.md)
+ [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) in the AWS OpsWorks CM API Reference
+ [https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/create-server.html](https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/create-server.html) in the *AWS CLI Command Reference*