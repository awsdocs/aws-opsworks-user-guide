# Stack Data Bag \(aws\_opsworks\_stack\)<a name="data-bag-json-stack"></a>

Represents a stack's settings\.

The following example shows how to use Chef search to write messages to the Chef log with the stack's name and cookbook's source url:

```
stack = search("aws_opsworks_stack").first
Chef::Log.info("********** The stack's name is '#{stack['name']}' **********")
Chef::Log.info("********** The stack's cookbook URL is '#{stack['custom_cookbooks_source']['url']}' **********")
```


****  

|  |  |  | 
| --- |--- |--- |
| [arn](#data-bag-json-stack-arn) | [custom\_cookbooks\_source](#data-bag-json-stack-cookbook-source) | [name](#data-bag-json-stack-name) | 
| [region](#data-bag-json-stack-region) | [stack\_id](#data-bag-json-stack-id) | [use\_custom\_cookbooks](#data-bag-json-stack-use-cookbooks) | 
| [vpc\_id](#data-bag-json-stack-vpc-id) |  |  | 

**arn**  <a name="data-bag-json-stack-arn"></a>
The stack's Amazon Resource Name \(ARN\) \(string\)\.

**custom\_cookbooks\_source**  <a name="data-bag-json-stack-cookbook-source"></a>
A set of content that specify the custom cookbook's source repository\.    
**type**  
The repository type \(string\)\. Valid values include:  
+ `"archive"`
+ `"git"`
+ `"s3"`  
**url**  
The repository URL, such as `"git://github.com/amazonwebservices/opsworks-demo-php-simple-app.git"` \(string\)\.  
**username**  
The user name for private repositories, and `null` for public repositories \(string\)\. For private Amazon Simple Storage Service \(Amazon S3\) buckets, the content is set to the access key\.  
**password**  
The password for private repositories, and `null` for public repositories \(string\)\. For private S3 buckets, this content is set to the secret key\.  
**ssh\_key**  
A [deploy SSH key](workingapps-deploykeys.md) for accessing private Git repositories, and `null` for public repositories \(string\)\.  
**revision**  
If the repository has multiple branches, the content specifies the app's branch or version, such as `"version1"` \(string\)\. Otherwise, it is set to `null`\.

**name**  <a name="data-bag-json-stack-name"></a>
The stack's name \(string\)\.

**region**  <a name="data-bag-json-stack-region"></a>
The stack's AWS region \(string\)\.

**stack\_id**  <a name="data-bag-json-stack-id"></a>
A GUID that identifies the stack \(string\)\.

**use\_custom\_cookbooks**  <a name="data-bag-json-stack-use-cookbooks"></a>
Whether custom cookbooks are enabled \(Boolean\)\.

**vpc\_id**  <a name="data-bag-json-stack-vpc-id"></a>
If the stack is running in a VPC, the VPC ID, if the stack is running in a VPC \(string\)\.