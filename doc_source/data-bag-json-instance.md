# Instance Data Bag \(aws\_opsworks\_instance\)<a name="data-bag-json-instance"></a>

Represents an instance's settings\.

The following example shows how to use Chef search to search through a single data bag item and then multiple data bag items to write messages to the Chef log with the instances' hostnames and IDs:

```
instance = search("aws_opsworks_instance").first
Chef::Log.info("********** The instance's hostname is '#{instance['hostname']}' **********")
Chef::Log.info("********** The instance's ID is '#{instance['instance_id']}' **********")

search("aws_opsworks_instance").each do |instance|
  Chef::Log.info("********** The instance's hostname is '#{instance['hostname']}' **********")
  Chef::Log.info("********** The instance's ID is '#{instance['instance_id']}' **********")
end
```

The following example shows different ways of using Chef search to search through multiple data bag items to find the data bag item that contains the specified Amazon EC2 instance ID\. The example then uses the data bag item's contents to write a message to the Chef log with the corresponding instance's public IP address:

```
instance = search("aws_opsworks_instance", "ec2_instance_id:i-12345678").first
Chef::Log.info("********** For instance '#{instance['ec2_instance_id']}', the instance's public IP address is '#{instance['public_ip']}' **********")
 
search("aws_opsworks_instance").each do |instance|
  if instance['ec2_instance_id'] == 'i-12345678'
    Chef::Log.info("********** For instance '#{instance['ec2_instance_id']}', the instance's public IP address is '#{instance['public_ip']}' **********")
  end
end
```

The following example shows how to use Chef search with `self:true` to find the data bag item that contains information related to the instance that the recipe is being executed on\. The example then uses the data bag item's contents to write a message to the Chef log with the corresponding instance's AWS OpsWorks Stacks\-generated ID and the instance's public IP address:

```
instance = search("aws_opsworks_instance", "self:true").first
Chef::Log.info("********** For instance '#{instance['instance_id']}', the instance's public IP address is '#{instance['public_ip']}' **********")
```


****  

|  |  |  | 
| --- |--- |--- |
| [ami\_id](#data-bag-json-instance-ami) | [architecture](#data-bag-json-instance-arch) | [auto\_scaling\_type](#data-bag-json-instance-autoscaling) | 
| [availability\_zone](#data-bag-json-instance-az) | [created\_at](#data-bag-json-instance-created-at) | [ebs\_optimized](#data-bag-json-instance-ebs-optimized) | 
| [ec2\_instance\_id](#data-bag-json-instance-ec2-id) | [elastic\_ip](#data-bag-json-instance-elastic-ip) | [hostname](#data-bag-json-instance-hostname) | 
| [instance\_id](#data-bag-json-instance-id) | [instance\_type](#data-bag-json-instance-type) | [layer\_ids](#data-bag-json-instance-layers) | 
| [os](#data-bag-json-instance-os) | [private\_dns](#data-bag-json-instance-private-dns) | [private\_ip](#data-bag-json-instance-private-ip) | 
| [public\_dns](#data-bag-json-instance-public-dns) | [public\_ip](#data-bag-json-instance-public-ip) | [root\_device\_type](#data-bag-json-instance-root-device-type) | 
| [root\_device\_volume\_id](#data-bag-json-instance-root-device-volume-id) | [self](#data-bag-json-instance-self) | [ssh\_host\_dsa\_key\_fingerprint](#data-bag-json-instance-ssh-host-dsa-key-fingerprint) | 
| [ssh\_host\_dsa\_key\_private](#data-bag-json-instance-ssh-host-dsa-key-private) | [ssh\_host\_dsa\_key\_public](#data-bag-json-instance-ssh-host-dsa-key-public) | [ssh\_host\_rsa\_key\_fingerprint](#data-bag-json-instance-ssh-host-rsa-key-fingerprint) | 
| [ssh\_host\_rsa\_key\_private](#data-bag-json-instance-ssh-host-rsa-key-private) | [ssh\_host\_rsa\_key\_public](#data-bag-json-instance-ssh-host-rsa-key-public) | [status](#data-bag-json-instance-status) | 
| [subnet\_id](#data-bag-json-instance-subnet-id) | [virtualization\_type](#data-bag-json-instance-virt-type) |  | 

**ami\_id**  <a name="data-bag-json-instance-ami"></a>
The instance's AMI \(Amazon Machine Image\) ID \(string\)\.

**architecture**  <a name="data-bag-json-instance-arch"></a>
The instance's architecture, which is always set to `"x86_64"` \(string\)\.

**auto\_scaling\_type**  <a name="data-bag-json-instance-autoscaling"></a>
The instance's scaling type: `null`, `timer`, or `load` \(string\)\.

**availability\_zone**  <a name="data-bag-json-instance-az"></a>
The instance's Availability Zone \(AZ\), such as `"us-west-2a"` \(string\)\.

**created\_at**  <a name="data-bag-json-instance-created-at"></a>
The time that the instance was created, using the UTC `"yyyy-mm-dddThh:mm:ss+hh:mm"` format \(string\)\. For example, `"2013-10-01T08:35:22+00:00"` corresponds to 8:35:22 on Oct\. 10, 2013, with no time zone offset\. For more information, see [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601)\.

**ebs\_optimized**  <a name="data-bag-json-instance-ebs-optimized"></a>
Whether the instance is EBS\-optimized \(Boolean\)\.

**ec2\_instance\_id**  <a name="data-bag-json-instance-ec2-id"></a>
The EC2 instance ID \(string\)\.

**elastic\_ip**  <a name="data-bag-json-instance-elastic-ip"></a>
The Elastic IP address; set to `"null"` if the instance does not have an Elastic IP address \(string\)\.

**hostname**  <a name="data-bag-json-instance-hostname"></a>
The host name, such as `"demo1"` \(string\)\.

**instance\_id**  <a name="data-bag-json-instance-id"></a>
The instance ID, which is an AWS OpsWorks Stacks\-generated GUID that uniquely identifies the instance \(string\)\.

**instance\_type**  <a name="data-bag-json-instance-type"></a>
The instance type, such as `"c1.medium"` \(string\)\.

**layer\_ids**  <a name="data-bag-json-instance-layers"></a>
A list of the instance's layers, identified by their unique IDs; for example, `307ut64c-c7e4-40cc-52f0-67d5k1f9992c`\.

**os**  <a name="data-bag-json-instance-os"></a>
The instance's operating system \(string\)\. Valid values include:  
+ `"Amazon Linux 2"`
+ `"Amazon Linux 2018.03"`
+ `"Amazon Linux 2017.09"`
+ `"Amazon Linux 2017.03"`
+ `"Amazon Linux 2016.09"`
+ `"Amazon Linux 2015.03"`
+ `"Amazon Linux 2015.09"`
+ `"Amazon Linux 2016.03"`
+ `"Custom"`
+ `"Microsoft Windows Server 2012 R2 Base"`
+ `"Microsoft Windows Server 2012 R2 with SQL Server Express"`
+ `"Microsoft Windows Server 2012 R2 with SQL Server Standard"`
+ `"Microsoft Windows Server 2012 R2 with SQL Server Web"`
+ `"CentOS 7"`
+ `"Red Hat Enterprise Linux 7"`
+ `"Ubuntu 12.04 LTS"`
+ `"Ubuntu 14.04 LTS"`
+ `"Ubuntu 16.04 LTS"`
+ `"Ubuntu 18.04 LTS"`

**private\_dns**  <a name="data-bag-json-instance-private-dns"></a>
The private DNS name \(string\)\.

**private\_ip**  <a name="data-bag-json-instance-private-ip"></a>
The private IP address \(string\)\.

**public\_dns**  <a name="data-bag-json-instance-public-dns"></a>
The public DNS name \(string\)\.

**public\_ip**  <a name="data-bag-json-instance-public-ip"></a>
The public IP address \(string\)\.

**root\_device\_type**  <a name="data-bag-json-instance-root-device-type"></a>
The root device type \(string\)\. Valid values include:  
+ `"ebs`
+ `"instance-store"`

**root\_device\_volume\_id**  <a name="data-bag-json-instance-root-device-volume-id"></a>
The root device's volume ID \(string\)\.

**self**  <a name="data-bag-json-instance-self"></a>
`true` if this data bag item contains information about the instance that the recipe is being executed on; otherwise, `false` \(Boolean\)\. This value is available only to recipes, not through the AWS OpsWorks Stacks API\.

**ssh\_host\_dsa\_key\_fingerprint**  <a name="data-bag-json-instance-ssh-host-dsa-key-fingerprint"></a>
A shorter sequence of bytes that identifies the longer DSA public key \(string\)\.

**ssh\_host\_dsa\_key\_private**  <a name="data-bag-json-instance-ssh-host-dsa-key-private"></a>
The DSA\-generated private key for SSH authentication with the instance \(string\)\.

**ssh\_host\_dsa\_key\_public**  <a name="data-bag-json-instance-ssh-host-dsa-key-public"></a>
The DSA\-generated public key for SSH authentication with the instance \(string\)\.

**ssh\_host\_rsa\_key\_fingerprint**  <a name="data-bag-json-instance-ssh-host-rsa-key-fingerprint"></a>
A shorter sequence of bytes that identifies the longer RSA public key \(string\)\.

**ssh\_host\_rsa\_key\_private**  <a name="data-bag-json-instance-ssh-host-rsa-key-private"></a>
The RSA\-generated private key for SSH authentication with the instance \(string\)\.

**ssh\_host\_rsa\_key\_public**  <a name="data-bag-json-instance-ssh-host-rsa-key-public"></a>
The RSA\-generated public key for SSH authentication with the instance \(string\)\.

**status**  <a name="data-bag-json-instance-status"></a>
The instance's status \(string\)\. Valid values include:  
+ `"requested"`
+ `"booting"`
+ `"running_setup"`
+ `"online"`
+ `"setup_failed"`
+ `"start_failed"`
+ `"terminating"`
+ `"terminated"`
+ `"stopped"`
+ `"connection_lost"`

**subnet\_id**  <a name="data-bag-json-instance-subnet-id"></a>
The instance's subnet ID \(string\)\.

**virtualization\_type**  <a name="data-bag-json-instance-virt-type"></a>
The instance's virtualization type \(string\)\.