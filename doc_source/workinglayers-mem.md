# Memcached<a name="workinglayers-mem"></a>

**Note**  
This layer is available only for for Chef 11 and earlier Linux\-based stacks\.

A Memcached layer is an AWS OpsWorks Stacks layer that provides a blueprint for instances that function as [Memcached](http://memcached.org/) servers—a distributed memory\-caching system for arbitrary data\. The Memcached layer includes the following configuration settings\.

**Allocated memory \(MB\)**  
\(Optional\) The amount of cache memory \(in MB\) for each of the layer's instances\. The default is 512 MB\.

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.