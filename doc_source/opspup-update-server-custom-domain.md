# Update an OpsWorks for Puppet Enterprise Server to Use a Custom Domain<a name="opspup-update-server-custom-domain"></a>

This section describes how to update an existing OpsWorks for Puppet Enterprise server to use a custom domain and certificate by using a backup of the server to create a new server\. Essentially, you are copying an existing OpsWorks for Puppet Enterprise 2\.0 server by creating a new server from a backup, then configuring the new server to use a custom domain, certificate, and private key\.

**Topics**
+ [Prerequisites](#opspup-update-server-custom-domain-reqs)
+ [Limitations](#opscm-update-server-custom-domain-limits)
+ [Update a Server to Use a Custom Domain](#opscm-update-server-custom-domain-howto)
+ [See Also](#opscm-update-server-custom-domain-seealso)

## Prerequisites<a name="opspup-update-server-custom-domain-reqs"></a>

The following are requirements for updating an existing OpsWorks for Puppet Enterprise server to use a custom domain and certificate\.
+ The server that you want to update \(or copy\) must be running Puppet Enterprise 2019\.8\.5\.
+ Decide which backup you want to use to create a new server\. You must have at least one backup available of the server that you want to update\. For more information about backups in OpsWorks for Puppet Enterprise, see [Back Up an OpsWorks for Puppet Enterprise Server](opspup-backup.md)\.
+ Have ready the service role and instance profile ARNs that you used to create the existing server that is the source of your backup\.
+ Be sure that you are running the most current release of the AWS CLI\. For more information about updating your AWS CLI tools, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) in the *AWS Command Line Interface User Guide*\.

## Limitations<a name="opscm-update-server-custom-domain-limits"></a>

When you update an existing server by creating a new server from a backup, the new server cannot be exactly the same as the existing OpsWorks for Puppet Enterprise server\.
+ You can only complete this procedure by using the AWS CLI or one of the [AWS SDKs](https://docs.aws.amazon.com/#sdks)\. You cannot create a new server from a backup by using the AWS Management Console\.
+ The new server cannot use the same name as the existing server within an account, and within an AWS Region\. The name must be different from the existing server that you used as the source of the backup\.
+ Nodes that were attached to the existing server are not managed by the new server\. You must do one of the following\.
  + Attach different nodes, because nodes cannot be managed by more than one Puppet master\.
  + Migrate the nodes from the existing server \(the source of the backup\) to the new server and the new custom domain endpoint\. For more information about how to migrate nodes, see the [Puppet Enterprise documentation](https://puppet.com/docs/pe/2019.8/backing_up_and_restoring_pe.html)\.

## Update a Server to Use a Custom Domain<a name="opscm-update-server-custom-domain-howto"></a>

To update an existing Puppet master, you make a copy of it by running the `create-server` command, adding parameters to specify a backup, a custom domain, a custom certificate, and a custom private key\.

1. If you do not have service role or instance profile ARNs available to specify in your `create-server` command, follow steps 1\-5 in [Create a Chef Automate server by using the AWS CLI](gettingstarted-opscm-create.md#gettingstarted-opscm-create-cli) to create a service role and instance profile that you can use\.

1. If you have not already done so, find the backup of the existing Puppet master on which you want to base a new server with a custom domain\. Run the following command to show information about all OpsWorks for Puppet Enterprise backups in your account, and in a region\. Be sure to note the ID of the backup that you want to use\.

   ```
   aws opsworks-cm --region region name describe-backups
   ```

1. Create the OpsWorks for Puppet Enterprise server by running the `create-server` command\.
   + The `--engine` value is `Puppet`, `--engine-model` is `Monolithic`, and `--engine-version` is `2019` or `2017`\.
   + The server name must be unique within your AWS account, within each region\. Server names must start with a letter; then letters, numbers, or hyphens \(\-\) are allowed, up to a maximum of 40 characters\.
   + Use the instance profile ARN and service role ARN that you copied in Steps 3 and 4\.
   + Valid instance types are `c4.large`, `c4.xlarge`, or `c4.2xlarge`\. For more information about the specifications of these instance types, see [Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon EC2 User Guide*\.
   + The `--engine-attributes` parameter is optional; if you don't specify a Puppet administrator password, the server creation process generates one for you\. If you add `--engine-attributes`, specify a `PUPPET_ADMIN_PASSWORD`, an administrator password for signing in to the Puppet Enterprise console webpage\. The password must use between 8 and 32 ASCII characters\.
   + An SSH key pair is optional, but can help you connect to your Puppet master if you need to reset the console administrator password\. For more information about creating an SSH key pair, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide*\.
   + To use a custom domain, add the following parameters to your command\. Otherwise, the Puppet master creation process automatically generates an endpoint for you\. All three parameters are required to configure a custom domain\. For information about additional requirements for using these parameters, see [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) in the AWS OpsWorks CM API Reference\.
     + `--custom-domain` \- An optional public endpoint of a server, such as `https://aws.my-company.com`\.
     + `--custom-certificate` \- A PEM\-formatted HTTPS certificate\. The value can be be a single, self\-signed certificate, or a certificate chain\.
     + `--custom-private-key` \- A private key in PEM format for connecting to the server by using HTTPS\. The private key must not be encrypted; it cannot be protected by a password or passphrase\.
   + Weekly system maintenance is required\. Valid values must be specified in the following format: `DDD:HH:MM`\. The specified time is in coordinated universal time \(UTC\)\. If you do not specify a value for `--preferred-maintenance-window`, the default value is a random, one\-hour period on Tuesday, Wednesday, or Friday\.
   + Valid values for `--preferred-backup-window` must be specified in one of the following formats: `HH:MM` for daily backups, or `DDD:HH:MM` for weekly backups\. The specified time is in UTC\. The default value is a random, daily start time\. To opt out of automatic backups, add the parameter `--disable-automated-backup` instead\.
   + For `--security-group-ids`, enter one or more security group IDs, separated by a space\.
   + For `--subnet-ids`, enter a subnet ID\.

   ```
   aws opsworks-cm create-server --engine "Puppet" --engine-model "Monolithic" --engine-version "2019" --server-name "server_name" --instance-profile-arn "instance_profile_ARN" --instance-type "instance_type" --engine-attributes '{"PUPPET_ADMIN_PASSWORD":"ASCII_password"}' --key-pair "key_pair_name" --preferred-maintenance-window "ddd:hh:mm" --preferred-backup-window "ddd:hh:mm" --security-group-ids security_group_id1 security_group_id2 --service-role-arn "service_role_ARN" --subnet-ids subnet_ID
   ```

   The following example creates a Puppet master that uses a custom domain\.

   ```
   aws opsworks-cm create-server \
       --engine "Puppet" \
       --engine-model "Monolithic" \
       --engine-version "2019" \
       --server-name "puppet-02" \
       --instance-profile-arn "arn:aws:iam::1019881987024:instance-profile/aws-opsworks-cm-ec2-role" \
       --instance-type "c4.large" \
       --engine-attributes '{"PUPPET_ADMIN_PASSWORD":"zZZzDj2DLYXSZFRv1d"}' \
       --custom-domain "my-puppet-master.my-corp.com" \
       --custom-certificate "-----BEGIN CERTIFICATE----- EXAMPLEqEXAMPLE== -----END CERTIFICATE-----" \
       --custom-private-key "-----BEGIN RSA PRIVATE KEY----- EXAMPLEqEXAMPLE= -----END RSA PRIVATE KEY-----" \
       --key-pair "amazon-test" 
       --preferred-maintenance-window "Mon:08:00" \
       --preferred-backup-window "Sun:02:00" \
       --security-group-ids sg-b00000001 sg-b0000008 \
       --service-role-arn "arn:aws:iam::044726508045:role/service-role/aws-opsworks-cm-service-role" \
       --subnet-ids subnet-383daa71
   ```

1. OpsWorks for Puppet Enterprise takes about 15 minutes to create a new server\. In the output of the `create-server` command, copy the value of the `Endpoint` attribute\. The following is an example\.

   ```
   "Endpoint": "puppet-2019-exampleexample.opsworks-cm.us-east-1.amazonaws.com"
   ```

   Do not dismiss the output of the `create-server` command or close your shell session, because the output can contain important information that is not shown again\. To get passwords and the starter kit from the `create-server` results, go on to the next step\.

1. If you opted to have OpsWorks for Puppet Enterprise generate a password for you, you can extract it in a usable format from the `create-server` results by using a JSON processor such as [jq](https://stedolan.github.io/jq/)\. After you install [jq](https://stedolan.github.io/jq/), you can run the following commands to extract the Puppet administrator password and starter kit\. If you did not provide your own password in Step 3, be sure to save the extracted administrator password in a convenient but secure location\.

   ```
   #Get the Puppet password:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "PUPPET_ADMIN_PASSWORD") | .Value'
   
   #Get the Puppet Starter Kit:
   cat resp.json | jq -r '.Server.EngineAttributes[] | select(.Name == "PUPPET_STARTER_KIT") | .Value' | base64 -D > starterkit.zip
   ```
**Note**  
You cannot regenerate a new Puppet master starter kit in the AWS Management Console\. When you create a Puppet master by using the AWS CLI, run the preceding `jq` command to save the base64\-encoded starter kit in the `create-server` results as a ZIP file\.

1. Optionally, if you did not extract the starter kit from `create-server` command results, you can download a new starter kit from the server's Properties page in the OpsWorks for Puppet Enterprise console\.

1. If you are not using a custom domain, go on to the next step\. If you are using a custom domain with the server, create a CNAME entry in your enterprise's DNS management tool to point your custom domain to the OpsWorks for Puppet Enterprise endpoint that you copied in step 4\. You cannot reach or sign in to a server with a custom domain until you complete this step\.

1. When the server creation process is finished, go on to [Configure the Puppet Master Using the Starter Kit](opspup-starterkit.md)\.

## See Also<a name="opscm-update-server-custom-domain-seealso"></a>
+ [Create a Puppet Enterprise Master by using the AWS CLI](gettingstarted-opspup-create.md#gettingstarted-opspup-create-cli)
+ [Back Up and Restore an OpsWorks for Puppet Enterprise Server](opspup-backup-restore.md)
+ [CreateServer](https://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_CreateServer.html) in the AWS OpsWorks CM API Reference
+ [https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/create-server.html](https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/create-server.html) in the *AWS CLI Command Reference*