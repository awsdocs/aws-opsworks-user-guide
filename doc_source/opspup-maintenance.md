# System Maintenance in OpsWorks for Puppet Enterprise<a name="opspup-maintenance"></a>

Mandatory system maintenance ensures that the latest AWS\-tested versions of Puppet Server, including security updates, are always running on an OpsWorks for Puppet Enterprise server\. System maintenance is required a minimum of once a week\. By using the AWS CLI, you can configure daily automatic maintenance, if desired\. You can also use the AWS CLI to perform system maintenance on demand, in addition to scheduled system maintenance\.

When new versions of Puppet software become available, system maintenance is designed to update the version of Puppet Server on the server automatically, as soon as it passes AWS testing\. AWS performs extensive testing to verify that Puppet upgrades are production\-ready and do not disrupt existing customer environments, so there can be lags between Puppet software releases and their availability for application to existing OpsWorks for Puppet Enterprise servers\. To update available versions of Puppet software on demand, see [Starting system maintenance on demand](#opspup-maintenance-startdemand) in this topic\.

System maintenance launches a new instance from a backup that is performed as part of the maintenance process, which helps reduce risk from degraded or impaired Amazon EC2 instances that undergo periodic maintenance\.

**Important**  
System maintenance deletes any files or custom configuration that you have added to the OpsWorks for Puppet Enterprise server\. For more information about how to repair configuration or file loss, see [Restoring custom configurations and files after maintenance](#opspup-maintenance-restore) in this topic\.

**Topics**
+ [Configuring system maintenance](#w4ab1b7c25c13)
+ [Starting system maintenance on demand](#opspup-maintenance-startdemand)
+ [Restoring custom configurations and files after maintenance](#opspup-maintenance-restore)

## Configuring system maintenance<a name="w4ab1b7c25c13"></a>

When you create a new OpsWorks for Puppet Enterprise server, you can configure a weekday and time, in [Coordinated Universal Time](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) \(UTC\), for system maintenance to start\. Maintenance starts during the hour that you specify\. Because you should expect the server to be offline during system maintenance, choose a time of low server demand within regular office hours\. The server status is `UNDER_MAINTENANCE` while maintenance is in progress\.

You can also change the system maintenance settings on an existing OpsWorks for Puppet Enterprise server, by changing settings in the **System maintenance** area of the **Settings** page for your server, as shown in the following screenshot\.

![\[Puppet master settings, showing System maintenance section.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_sysmaint_exist.png)

In the **System maintenance** section, set the day and hour that you want system maintenance to begin\.

### Configuring system maintenance by using the AWS CLI<a name="w4ab1b7c25c13c10"></a>

You can also configure the system maintenance automatic start time by using the AWS CLI\. The AWS CLI lets you configure daily automatic maintenance, if desired, by omitting the three\-character weekday prefix\.

In a `create-server` command, add the `--preferred-maintenance-window` parameter to your command, after specifying the requirements for creating the server instance \(such as instance type, instance profile ARN, and service role ARN\)\. In the following `create-server` example, `--preferred-maintenance-window` is set to `Mon:08:00`, meaning that you've set maintenance to start every Monday morning at 8:00 a\.m\. UTC\.

```
aws opsworks-cm create-server --engine "Puppet" --engine-model "Monolithic" --engine-version "2017" --server-name "puppet-06" --instance-profile-arn "arn:aws:iam::1119001987000:instance-profile/aws-opsworks-cm-ec2-role" --instance-type "c4.large" --key-pair "amazon-test" --service-role-arn "arn:aws:iam::044726508045:role/aws-opsworks-cm-service-role" --preferred-maintenance-window "Mon:08:00"
```

In an `update-server` command, you can update the `--preferred-maintenance-window` value alone, if desired\. In the following example, the maintenance window is set to Friday night at 6:15 p\.m\. UTC\.

```
aws opsworks-cm update-server --server-name "puppet-06" --preferred-maintenance-window "Fri:18:15"
```

To change the start time of the maintenance window to 6:15 p\.m\. UTC every day, omit the three\-character weekday prefix, as shown in the following example\.

```
aws opsworks-cm update-server --server-name "puppet-06" --preferred-maintenance-window "18:15"
```

For more information about setting the preferred system maintenance window by using the AWS CLI, see [create\-server](http://docs.aws.amazon.com/cli/latest/reference/opsworkscm/update-server.html) and [update\-server](http://docs.aws.amazon.com/cli/latest/reference/opsworkscm/update-server.html)\.

## Starting system maintenance on demand<a name="opspup-maintenance-startdemand"></a>

To start system maintenance on demand, outside of your configured weekly or daily automatic maintenance, run the following AWS CLI command\. You cannot start on\-demand maintenance in the AWS Management Console\.

```
aws opsworks-cm start-maintenance --server-name server_name
```

For more information about this command, see [start\-maintenance](http://docs.aws.amazon.com/cli/latest/reference/opsworkscm/start-maintenance.html)\.

## Restoring custom configurations and files after maintenance<a name="opspup-maintenance-restore"></a>

System maintenance can delete or change custom files or configurations that you have added to your OpsWorks for Puppet Enterprise server\.

If, after a maintenance run, your Puppet master is missing files or settings that you added by using `RunCommand` or SSH, you can use an Amazon Machine Image \(AMI\) to launch a new Amazon EC2 instance\. AMIs are available that are built from a server's pre\-maintenance configuration\.

The new instance is in the same state that the Puppet master was before maintenance, and should include your missing files and settings\.

**Important**  
You cannot use the new instance to restore your server; the instance cannot be run as a Puppet master\. You can use the instance only to recover your files and configuration settings\.

To launch an EC2 instance from an AMI, in the Amazon EC2 console, open the **Launch** wizard, choose **My AMIs**, and then choose the AMI that has your server name\. Follow Amazon EC2 wizard steps as you would for any other instance launch\.