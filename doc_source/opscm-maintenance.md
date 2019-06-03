# System Maintenance in AWS OpsWorks for Chef Automate<a name="opscm-maintenance"></a>

Mandatory system maintenance ensures that the latest minor versions of Chef Server and Chef Automate Server, including security updates, are always running on an AWS OpsWorks for Chef Automate server\. System maintenance is required a minimum of once a week\. By using the AWS CLI, you can configure daily automatic maintenance, if desired\. You can also use the AWS CLI to perform system maintenance on demand, in addition to scheduled system maintenance\.

When new minor versions of Chef software become available, system maintenance is designed to update the minor version of Chef Automate and Chef Server on the server automatically, as soon as it passes AWS testing\. AWS performs extensive testing to verify that Chef upgrades are production\-ready and do not disrupt existing customer environments, so there can be lags between Chef software releases and their availability for application to existing OpsWorks for Chef Automate servers\. To update available minor versions of Chef software on demand, see [Starting system maintenance on demand](#opscm-maintenance-startdemand) in this topic\.

System maintenance launches a new instance from a backup that is performed as part of the maintenance process, which helps reduce risk from degraded or impaired Amazon EC2 instances that undergo periodic maintenance\.

**Important**  
System maintenance deletes any files or custom configuration that you have added to the AWS OpsWorks for Chef Automate server\. For more information about how to repair configuration or file loss, see [Restoring custom configurations and files after maintenance](#opscm-maintenance-restore) in this topic\.

**Topics**
+ [Ensuring nodes trust the AWS OpsWorks Certification Authority](#w4ab1b9c29c13)
+ [Configuring system maintenance](#w4ab1b9c29c15)
+ [Starting system maintenance on demand](#opscm-maintenance-startdemand)
+ [Restoring custom configurations and files after maintenance](#opscm-maintenance-restore)

## Ensuring nodes trust the AWS OpsWorks Certification Authority<a name="w4ab1b9c29c13"></a>

Nodes that you are managing with an AWS OpsWorks for Chef Automate server must authenticate with the server by using certificates\. During system maintenance, AWS OpsWorks replaces the server instance, and regenerates new certificates through the AWS OpsWorks certificate authority \(CA\)\. To restore communication automatically with your managed nodes after maintenance is finished, nodes should trust the AWS OpsWorks CA that ships with the starter kit, and is hosted in the regions that are supported by AWS OpsWorks for Chef Automate\. When you use the AWS OpsWorks CA to establish the trust between nodes and server, nodes reconnect to the new server instance after maintenance\. If you are adding EC2 nodes by using the EC2 `userdata` script described in [Adding Nodes Automatically in AWS OpsWorks for Chef Automate](opscm-unattend-assoc.md), nodes are already configured to trust the AWS OpsWorks CA\.
+ For Linux\-based nodes, the S3 bucket location of the CA is `https://opsworks-cm-${REGION}-prod-default-assets.s3.amazonaws.com/misc/opsworks-cm-ca-2016-root.pem`\. The AWS OpsWorks trusted CA must be stored in the path `/etc/chef/opsworks-cm-ca-2016-root.pem`\.
+ For Windows\-based nodes, the S3 bucket location of the CA is `https://opsworks-cm-$env:AWS_REGION-prod-default-assets.s3.amazonaws.com/misc/opsworks-cm-ca-2016-root.pem`\. The AWS OpsWorks CA must be stored in the root Chef folder; for example, `C:\chef\opsworks-cm-ca-2016-root.pem`

In both paths, the region variable resolves to one of the following\.
+ `us-east-2`
+ `us-east-1`
+ `us-west-1`
+ `us-west-2`
+ `ap-northeast-1`
+ `ap-southeast-1`
+ `ap-southeast-2`
+ `eu-central-1`
+ `eu-west-1`

## Configuring system maintenance<a name="w4ab1b9c29c15"></a>

When you create a new AWS OpsWorks for Chef Automate server, you can configure a weekday and time, in [Coordinated Universal Time](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) \(UTC\), for system maintenance to start\. Maintenance starts during the hour that you specify\. Because you should expect the server to be offline during system maintenance, choose a time of low server demand within regular office hours\. The server status is `UNDER_MAINTENANCE` while maintenance is in progress\.

You can also change the system maintenance settings on an existing AWS OpsWorks for Chef Automate server, by changing settings in the **System maintenance** area of the **Settings** page for your server, as shown in the following screenshot\.

![\[Chef Automate Server settings, showing System maintenance section.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opscm_exist_update_maintenance.png)

In the **System maintenance** section, set the day and hour that you want system maintenance to begin\.

### Configuring system maintenance by using the AWS CLI<a name="w4ab1b9c29c15c10"></a>

You can also configure the system maintenance automatic start time by using the AWS CLI\. The AWS CLI lets you configure daily automatic maintenance, if desired, by omitting the three\-character weekday prefix\.

In a `create-server` command, add the `--preferred-maintenance-window` parameter to your command, after specifying the requirements for creating the server instance \(such as instance type, instance profile ARN, and service role ARN\)\. In the following `create-server` example, `--preferred-maintenance-window` is set to `Mon:08:00`, meaning that you've set maintenance to start every Monday morning at 8:00 a\.m\. UTC\.

```
aws opsworks-cm create-server --engine "Chef" --engine-model "Single" --engine-version "12" --server-name "automate-06" --instance-profile-arn "arn:aws:iam::1019881987024:instance-profile/aws-opsworks-cm-ec2-role" --instance-type "t2.medium" --key-pair "amazon-test" --service-role-arn "arn:aws:iam::044726508045:role/aws-opsworks-cm-service-role" --preferred-maintenance-window "Mon:08:00"
```

In an `update-server` command, you can update the `--preferred-maintenance-window` value alone, if desired\. In the following example, the maintenance window is set to Friday night at 6:15 p\.m\. UTC\.

```
aws opsworks-cm update-server --server-name "shiny-kitchen" --preferred-maintenance-window "Fri:18:15"
```

To change the start time of the maintenance window to 6:15 p\.m\. UTC every day, omit the three\-character weekday prefix, as shown in the following example\.

```
aws opsworks-cm update-server --server-name "shiny-kitchen" --preferred-maintenance-window "18:15"
```

For more information about setting the preferred system maintenance window by using the AWS CLI, see [create\-server](http://docs.aws.amazon.com/cli/latest/reference/opsworkscm/update-server.html) and [update\-server](http://docs.aws.amazon.com/cli/latest/reference/opsworkscm/update-server.html)\.

## Starting system maintenance on demand<a name="opscm-maintenance-startdemand"></a>

To start system maintenance on demand, outside of your configured weekly or daily automatic maintenance, run the following AWS CLI command\. You cannot start on\-demand maintenance in the AWS Management Console\.

```
aws opsworks-cm start-maintenance --server-name server_name
```

For more information about this command, see [start\-maintenance](http://docs.aws.amazon.com/cli/latest/reference/opsworkscm/start-maintenance.html)\.

## Restoring custom configurations and files after maintenance<a name="opscm-maintenance-restore"></a>

System maintenance can delete or change custom files or configurations that you have added to your AWS OpsWorks for Chef Automate server\.

If, after a maintenance run, your Chef server is missing files or settings that you added by using `RunCommand` or SSH, you can use an Amazon Machine Image \(AMI\) to launch a new Amazon EC2 instance\. AMIs are available that are built from a server's pre\-maintenance configuration\.

The new instance is in the same state that the Chef server was before maintenance, and should include your missing files and settings\.

**Important**  
You cannot use the new instance to restore your server; the instance cannot be run as a Chef server\. You can use the instance only to recover your files and configuration settings\.

To launch an EC2 instance from an AMI, in the Amazon EC2 console, open the **Launch** wizard, choose **My AMIs**, and then choose the AMI that has your server name\. Follow Amazon EC2 wizard steps as you would for any other instance launch\.