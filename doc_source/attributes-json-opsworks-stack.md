# stack Attributes<a name="attributes-json-opsworks-stack"></a>

`stack` attributes specify some aspects of the stack configuration, such as service layer configurations\.
+ [elb\-load\-balancers](#attributes-json-opsworks-stack-elb)
+ [id](#attributes-json-opsworks-stack-id)
+ [name](#attributes-json-opsworks-stack-name)
+ [rds\_instances](#attributes-json-opsworks-stack-rds)
+ [vpc\_id](#attributes-json-opsworks-stack-vpc-id)

**elb\-load\-balancers**  <a name="attributes-json-opsworks-stack-elb"></a>
Contains a list of embedded objects, one for each Elastic Load Balancing load balancer in the stack\. Each embedded object contains the following attributes that describe the load balancer configuration\.  
The general node syntax for these attributes is as follows, where `i` specifies the instance's zero\-based list index\.  

```
node["opsworks"]["stack"]["elb-load-balancers"]["i"]["attribute_name"]
```  
**dns\_name**  <a name="attributes-json-opsworks-stack-elb-dns-name"></a>
The load balancer's DNS name \(string\)\.  

```
node["opsworks"]["stack"]["elb-load-balancers"]["i"]["dns_name"]
```  
**name**  <a name="attributes-json-opsworks-stack-elb-name"></a>
The load balancer's name \(string\)\.  

```
node["opsworks"]["stack"]["elb-load-balancers"]["i"]["name"]
```  
**layer\_id**  <a name="attributes-json-opsworks-stack-elb-layer-id"></a>
The ID of the layer that the load balancer is attached to \(string\)\.  

```
node["opsworks"]["stack"]["elb-load-balancers"]["i"]["layer_id"]
```

**id**  <a name="attributes-json-opsworks-stack-id"></a>
The stack ID \(string\)\.  

```
node["opsworks"]["stack"]["id"]
```

**name**  <a name="attributes-json-opsworks-stack-name"></a>
The stack name \(string\)\.  

```
node["opsworks"]["stack"]["name"]
```

**rds\_instances**  <a name="attributes-json-opsworks-stack-rds"></a>
Contains a list of embedded objects, one for each Amazon RDS instance that is registered with the stack\. Each embedded object contains a set of attributes that define the instance's configuration\. You specify these values when you use the Amazon RDS console or API to create the instance\. You can also use the Amazon RDS console or API to edit some of the settings after the instance has been created\. For more information, see the [Amazon RDS documentation](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)\.  
The general node syntax for these attributes is as follows, where `i` specifies the instance's zero\-based list index\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["attribute_name"]
```
If your stack has multiple Amazon RDS instances, the following is an example of how to use a particular instance in a recipe\.  

```
if my_rds = node["opsworks"]["stack"]["rds_instances"].select{|rds_instance| rds_instance["db_instance_identifier"] == ‘db_id’ }.first
  template “/etc/rds.conf” do
    source "rds.conf.erb"
    variables :address => my_rds["address"]
  end
end
```  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/attributes-json-opsworks-stack.html)  
**address**  <a name="attributes-json-opsworks-stack-rds-address"></a>
The instances URL, such as `opsinstance.ccdvt3hwog1a.us-west-2.rds.amazonaws.com` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["address"]
```  
**allocated\_storage**  <a name="attributes-json-opsworks-stack-rds-storage"></a>
The allocated storage, in GB \(number\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["allocated_storage"]
```  
**arn**  <a name="attributes-json-opsworks-stack-rds-arn"></a>
The instance's ARN \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["arn"]
```  
**auto\_minor\_version\_upgrade**  <a name="attributes-json-opsworks-stack-rds-minor"></a>
Whether to automatically apply minor version upgrades \(Boolean\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["auto_minor_version_upgrade"]
```  
**availability\_zone**  <a name="attributes-json-opsworks-stack-rds-az"></a>
The instance's Availability Zone, such as `us-west-2a` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["availability_zone"]
```  
**backup\_retention\_period**  <a name="attributes-json-opsworks-stack-rds-backup"></a>
The backup retention period, in days \(number\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["backup_retention_period"]
```  
**db\_instance\_class**  <a name="attributes-json-opsworks-stack-rds-db-class"></a>
The DB instance class, such as `db.m1.small` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_instance_class"]
```  
**db\_instance\_identifier**  <a name="attributes-json-opsworks-stack-rds-db-id"></a>
The user\-defined DB instance identifier \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_instance_identifier"]
```  
**db\_instance\_status**  <a name="attributes-json-opsworks-stack-rds-db-status"></a>
The instance's status \(string\)\. For more information, see [DB Instance](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.DBInstance.html)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_instance_status"]
```  
**db\_name**  <a name="attributes-json-opsworks-stack-rds-db-name"></a>
The user\-defined DB name \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_name"]
```  
**db\_parameter\_groups**  <a name="attributes-json-opsworks-stack-rds-db-param"></a>
The instance's DB parameter groups, which contains a list of embedded objects, one for each parameter group\. For more information, see [Working with DB Parameter Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html)\. Each object contains the following attributes:    
**db\_parameter\_group\_name**  <a name="attributes-json-opsworks-stack-rds-db-param-name"></a>
The group name \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_parameter_groups"][j"]["db_parameter_group_name"]
```  
**parameter\_apply\_status**  <a name="attributes-json-opsworks-stack-rds-db-param-status"></a>
The apply status \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_parameter_groups"][j"]["parameter_apply_status"]
```  
**db\_security\_groups**  <a name="attributes-json-opsworks-stack-rds-db-security"></a>
The instance's database security groups, which contains a list of embedded objects, one for each security group\. For more information, see [Working with DB Security Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html)\. Each object contains the following attributes    
**db\_security\_group\_name**  <a name="attributes-json-opsworks-stack-rds-db-security-name"></a>
The security group name \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_security_groups"][j"]["db_security_group_name"]
```  
**status**  <a name="attributes-json-opsworks-stack-rds-db-security-status"></a>
The status \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_security_groups"][j"]["status"]
```  
**db\_user**  <a name="attributes-json-opsworks-stack-rds-db-user"></a>
The user\-defined Master User name \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["db_user"]
```  
**engine**  <a name="attributes-json-opsworks-stack-rds-engine"></a>
The database engine, such as `mysql(5.6.13)` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["engine"]
```  
**instance\_create\_time**  <a name="attributes-json-opsworks-stack-rds-create"></a>
The instance creation time, such as `2014-04-15T16:13:34Z` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["instance_create_time"]
```  
**license\_model**  <a name="attributes-json-opsworks-stack-rds-license"></a>
The instance's license model, such as `general-public-license` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["license_model"]
```  
**multi\_az**  <a name="attributes-json-opsworks-stack-rds-multi-az"></a>
Whether multi\-AZ deployment is enabled \(Boolean\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["multi_az"]
```  
**option\_group\_memberships**  <a name="attributes-json-opsworks-stack-rds-option"></a>
The instance's option group memberships, which contains a list of embedded objects, one for each option group\. For more information, see [Working with Option Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html)\. Each object contains the following attributes:    
**option\_group\_name**  <a name="attributes-json-opsworks-stack-rds-option-name"></a>
The group's name \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["option_group_memberships"][j"]["option_group_name"]
```  
**status**  <a name="attributes-json-opsworks-stack-rds-option-status"></a>
The group's status \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["option_group_memberships"][j"]["status"]
```  
**port**  <a name="attributes-json-opsworks-stack-rds-port"></a>
The database server's port \(number\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["port"]
```  
**preferred\_backup\_window**  <a name="attributes-json-opsworks-stack-rds-backup-window"></a>
The preferred daily backup window, such as `06:26-06:56` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["preferred_backup_window"]
```  
**preferred\_maintenance\_window**  <a name="attributes-json-opsworks-stack-rds-maintenance"></a>
The preferred weekly maintenance window, such as `thu:07:13-thu:07:43` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["preferred_maintenance_window"]
```  
**publicly\_accessible**  <a name="attributes-json-opsworks-stack-rds-public"></a>
Whether the database is publicly accessible \(Boolean\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["publicly_accessible"]
```  
**read\_replica\_db\_instance\_identifiers**  <a name="attributes-json-opsworks-stack-rds-read"></a>
A list of the read\-replica instance identifiers \(list of string\)\. For more information, see [Working with Read Replicas](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["read_replica_db_instance_identifiers"]
```  
**region**  <a name="attributes-json-opsworks-stack-rds-region"></a>
The AWS region, such as `us-west-2` \(string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["region"]
```  
**status\_infos**  <a name="attributes-json-opsworks-stack-rds-status"></a>
A list of status information \(list of string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["status_infos"]
```  
**vpc\_security\_groups**  <a name="attributes-json-opsworks-stack-rds-vpc-sec"></a>
A list of VPC security groups \(list of string\)\.  

```
node["opsworks"]["stack"]["rds_instances"]["i"]["vpc_security_groups"]
```

**vpc\_id**  <a name="attributes-json-opsworks-stack-vpc-id"></a>
The VPC id \(string\)\. This value is `null` if the instance is not in a VPC\.  

```
node["opsworks"]["stack"]["vpc_id"]
```