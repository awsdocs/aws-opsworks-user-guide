# Amazon RDS Data Bag \(aws\_opsworks\_rds\_db\_instance\)<a name="data-bag-json-rds"></a>

A set of data bag content that specifies an Amazon Relational Database Service \(Amazon RDS\) instance's configuration as follows:


****  

|  |  |  | 
| --- |--- |--- |
| [address](#data-bag-json-rds-address) | [db\_instance\_identifier](#data-bag-json-rds-id) | [db\_password](#data-bag-json-rds-password) | 
| [db\_user](#data-bag-json-rds-user) | [engine](#data-bag-json-rds-engine) | [rds\_db\_instance\_arn](#data-bag-json-rds-arn) | 
| [region](#data-bag-json-rds-region) |  |  | 

The following example shows how to use Chef search to search through a single data bag item and then multiple data bag items to write messages to the Chef log with the Amazon RDS instances' addresses and database engine types:

```
rds_db_instance = search("aws_opsworks_rds_db_instance").first
Chef::Log.info("********** The RDS instance's address is '#{rds_db_instance['address']}' **********")
Chef::Log.info("********** The RDS instance's database engine type is '#{rds_db_instance['engine']}' **********")

search("aws_opsworks_rds_db_instance").each do |rds_db_instance|
  Chef::Log.info("********** The RDS instance's address is '#{rds_db_instance['address']}' **********")
  Chef::Log.info("********** The RDS instance's database engine type is '#{rds_db_instance['engine']}' **********")
end
```

**address**  
The instance's DNS name\.

**port**  
The instance's port\.

**db\_instance\_identifier**  
The instance's ID\.

**db\_password**  
The instance's master password\.

**db\_user**  
The instance's master user name\.

**engine**  
The instance's database engine, such as `mysql`\.

**rds\_db\_instance\_arn**  
The instance's Amazon Resource Name \(ARN\)\.

**region**  
The instance's AWS region, such as `us-west-2`\.