# Reset Chef Automate Dashboard Credentials<a name="opscm-resetchefcreds"></a>

Periodically, you might want to change the password with which you sign in to the Chef Automate dashboard\. You can also use the Amazon EC2 Systems Manager AWS CLI command that's documented in this section to change the Chef Automate dashboard password if you have lost it\. 

1. To return the instance ID of your Chef server, open the AWS Management Console to the following page\. 

   https://console\.aws\.amazon\.com/ec2/v2/home?region=*region\_of\_your\_server*\#Instances:search=aws\-opsworks\-cm\-*server\_name*

   For example, for a Chef server named **MyChefServer** in the US West \(Oregon\) Region, the console URL would be the following\.

   https://console\.aws\.amazon\.com/ec2/v2/home?region=us\-west\-2\#Instances:search=aws\-opsworks\-cm\-MyChefServer

   Make a note of the instance ID that is displayed in the console; you will need it to change your password\.

1. To reset the Chef Automate dashboard sign\-in password, run the following AWS CLI command\. Replace *enterprise\_name* with your enterprise or organization name, *user\_name* with the IAM user name of an administrator on the server, *new\_password* with the password you want to use, and*region\_name* with the region in which your server is located\. If you do not specify an enterprise name, the enterprise name will be `default`\. By default, *enterprise\_name* is `default` \(this is the name of the organization that is always provisioned\)\. For *user\_name*, AWS OpsWorks for Chef Automate only creates a user named `admin`\. Make a note of the new password, and store it in a safe but convenient location\.

   ```
   aws ssm send-command --document-name "AWS-RunShellScript" --comment "reset admin password" --instance-ids "instance_id" 
   --parameters commands="sudo delivery-ctl reset-password enterprise_name user_name new_password" --region region_name --output text
   ```

1. Wait for output text \(in this case, the command ID\) to show that the password change is finished\.

For more information about working with the Chef Automate dashboard, see [An Overview of Visibility in Chef Automate](https://docs.chef.io/visibility.html)\.