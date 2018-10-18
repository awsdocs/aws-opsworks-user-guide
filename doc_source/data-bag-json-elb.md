# Elastic Load Balancing Data Bag \(aws\_opsworks\_elastic\_load\_balancer\)<a name="data-bag-json-elb"></a>

Represents an Elastic Load Balancing load balancer's settings\.

The following example shows how to use Chef search to search through a single data bag item and then multiple data bag items to write messages to the Chef log with the Elastic Load Balancing load balancers' names and DNS names:

```
elastic_load_balancer = search("aws_opsworks_elastic_load_balancer").first
Chef::Log.info("********** The ELB's name is '#{elastic_load_balancer['elastic_load_balancer_name']}' **********")
Chef::Log.info("********** The ELB's DNS name is '#{elastic_load_balancer['dns_name']}' **********")

search("aws_opsworks_elastic_load_balancer").each do |elastic_load_balancer|
  Chef::Log.info("********** The ELB's name is '#{elastic_load_balancer['elastic_load_balancer_name']}' **********")
  Chef::Log.info("********** The ELB's DNS name is '#{elastic_load_balancer['dns_name']}' **********")
end
```


****  

|  |  |  | 
| --- |--- |--- |
| [elastic\_load\_balancer\_name](#data-bag-json-elb-elastic-load-balancer-name) | [dns\_name](#data-bag-json-elb-dns-name) | [layer\_id](#data-bag-json-elb-layer-id) | 

**elastic\_load\_balancer\_name**  <a name="data-bag-json-elb-elastic-load-balancer-name"></a>
The load balancer's name \(string\)\.

**dns\_name**  <a name="data-bag-json-elb-dns-name"></a>
The load balancer's DNS name \(string\)\.

**layer\_id**  <a name="data-bag-json-elb-layer-id"></a>
The AWS OpsWorks Stacks ID of the layer that the load balancer is assigned to \(string\)\.