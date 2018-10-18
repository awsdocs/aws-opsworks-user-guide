# instance Attributes<a name="attributes-json-opsworks-instance"></a>

The `instance` attribute contains a set of attributes that specify the configuration of this instance\.


****  

|  |  |  | 
| --- |--- |--- |
| [architecture](#attributes-json-opsworks-instance-arch) | [availability\_zone](#attributes-json-opsworks-instance-availability) | [backends](#attributes-json-opsworks-instance-backends) | 
| [aws\_instance\_id](#attributes-json-opsworks-instance-ec2-id) | [hostname](#attributes-json-opsworks-instance-host) | [id](#attributes-json-opsworks-instance-id) | 
| [instance\_type](#attributes-json-opsworks-instance-type) | [ip](#attributes-json-opsworks-instance-ip) | [layers](#attributes-json-opsworks-instance-layers) | 
| [private\_dns\_name](#attributes-json-opsworks-instance-private-dns) | [private\_ip](#attributes-json-opsworks-instance-private-ip) | [public\_dns\_name](#attributes-json-opsworks-instance-dns) | 
| [region](#attributes-json-opsworks-instance-region) |  |  | 

**architecture**  <a name="attributes-json-opsworks-instance-arch"></a>
The instance's architecture, such as `"i386"` \(string\)\.  

```
node["opsworks"]["instance"]["architecture"]
```

**availability\_zone**  <a name="attributes-json-opsworks-instance-availability"></a>
The instance's availability zone, such as `"us-west-2a"` \(string\)\.  

```
node["opsworks"]["instance"]["availability_zone"]
```

**backends**  <a name="attributes-json-opsworks-instance-backends"></a>
The number of back\-end web processes \(string\)\. It determines, for example, the number of concurrent connections that HAProxy will forward to a Rails back end\. The default value depends on the instance's memory and number of cores\.  

```
node["opsworks"]["instance"]["backends"]
```

**aws\_instance\_id**  <a name="attributes-json-opsworks-instance-ec2-id"></a>
The EC2 instance ID \(string\)\.  

```
node["opsworks"]["instance"]["aws_instance_id"]
```

**hostname**  <a name="attributes-json-opsworks-instance-host"></a>
The host name, such as `"php-app1"` \(string\)\.  

```
node["opsworks"]["instance"]["hostname"]
```

**id**  <a name="attributes-json-opsworks-instance-id"></a>
The instance ID, which is an AWS OpsWorks Stacks\-generated GUID that uniquely identifies the instance \(string\)\.  

```
node["opsworks"]["instance"]["id"]
```

**instance\_type**  <a name="attributes-json-opsworks-instance-type"></a>
The instance type, such as `"c1.medium"` \(string\)\.  

```
node["opsworks"]["instance"]["instance_type"]
```

**ip**  <a name="attributes-json-opsworks-instance-ip"></a>
The public IP address \(string\)\.  

```
node["opsworks"]["instance"]["ip"]
```

**layers**  <a name="attributes-json-opsworks-instance-layers"></a>
A list of the instance's layers, which are identified by their short names, such as `"lb"` or `"db-master"` \(list of string\)\.  

```
node["opsworks"]["instance"]["layers"]
```

**private\_dns\_name**  <a name="attributes-json-opsworks-instance-private-dns"></a>
The private DNS name \(string\)\.  

```
node["opsworks"]["instance"]["private_dns_name"]
```

**private\_ip**  <a name="attributes-json-opsworks-instance-private-ip"></a>
The private IP address \(string\)\.  

```
node["opsworks"]["instance"]["private_ip"]
```

**public\_dns\_name**  <a name="attributes-json-opsworks-instance-dns"></a>
The public DNS name \(string\)\.  

```
node["opsworks"]["instance"]["public_dns_name"]
```

**region**  <a name="attributes-json-opsworks-instance-region"></a>
The AWS region, such as `"us-west-2"` \(string\)\.  

```
node["opsworks"]["instance"]["region"]
```