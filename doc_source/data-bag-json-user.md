# User Data Bag \(aws\_opsworks\_user\)<a name="data-bag-json-user"></a>

Represents a user's settings\.

The following example shows how to use Chef search to search through a single data bag item and then multiple data bag items to write messages to the Chef log with the users' user names and Amazon Resource Names \(ARNs\):

```
user = search("aws_opsworks_user").first
Chef::Log.info("********** The user's user name is '#{user['username']}' **********")
Chef::Log.info("********** The user's user ARN is '#{user['iam_user_arn']}' **********")

# Or...

search("aws_opsworks_user").each do |user|
  Chef::Log.info("********** The user's user name is '#{user['username']}' **********")
  Chef::Log.info("********** The user's user ARN is '#{user['iam_user_arn']}' **********")
end
```


****  

|  |  |  | 
| --- |--- |--- |
| [administrator\_privileges](#data-bag-json-user-admin) | [iam\_user\_arn](#data-bag-json-user-arn) | [remote\_access](#data-bag-json-user-rdp) | 
| [ssh\_public\_key](#data-bag-json-user-ssh-public-key) | [unix\_user\_id](#data-bag-json-user-unix-id) | [username](#data-bag-json-user-username) | 

**administrator\_privileges**  <a name="data-bag-json-user-admin"></a>
Whether the user has administrator privileges \(Boolean\)\.

**iam\_user\_arn**  <a name="data-bag-json-user-arn"></a>
The user's Amazon Resource Name \(ARN\) \(string\)\. 

**remote\_access**  <a name="data-bag-json-user-rdp"></a>
Whether the user can use RDP to log in to the instance \(Boolean\)\.

**ssh\_public\_key**  <a name="data-bag-json-user-ssh-public-key"></a>
The user's public key, as provided through the AWS OpsWorks Stacks console or API \(string\)\.

**unix\_user\_id**  <a name="data-bag-json-user-unix-id"></a>
The user's Unix ID \(number\)\.

**username**  <a name="data-bag-json-user-username"></a>
The user name \(string\)\.