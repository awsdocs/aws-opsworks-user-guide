# Amazon ECS Cluster Data Bag \(aws\_opsworks\_ecs\_cluster\)<a name="data-bag-json-ecs-cluster"></a>

Represents an Amazon ECS cluster's settings\.

The following example shows how to use Chef search to search through a single data bag item and then multiple data bag items to write messages to the Chef log with the Amazon ECS clusters' names and Amazon Resource Names \(ARNs\):

```
ecs_cluster = search("aws_opsworks_ecs_cluster").first
Chef::Log.info("********** The ECS cluster's name is '#{ecs_cluster['ecs_cluster_name']}' **********")
Chef::Log.info("********** The ECS cluster's ARN is '#{ecs_cluster['ecs_cluster_arn']}' **********")

search("aws_opsworks_ecs_cluster").each do |ecs_cluster|
  Chef::Log.info("********** The ECS cluster's name is '#{ecs_cluster['ecs_cluster_name']}' **********")
  Chef::Log.info("********** The ECS cluster's ARN is '#{ecs_cluster['ecs_cluster_arn']}' **********")
end
```


****  

|  |  | 
| --- |--- |
| [ecs\_cluster\_arn](#data-bag-json-ecs-cluster-ecs-cluster-arn) | [ecs\_cluster\_name](#data-bag-json-ecs-cluster-ecs-cluster-name) | 

**ecs\_cluster\_arn**  <a name="data-bag-json-ecs-cluster-ecs-cluster-arn"></a>
The cluster's Amazon Resource Name \(ARN\) \(string\)\.

**ecs\_cluster\_name**  <a name="data-bag-json-ecs-cluster-ecs-cluster-name"></a>
The cluster's name \(string\)\.