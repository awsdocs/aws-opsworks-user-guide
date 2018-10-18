# Command Data Bag \(aws\_opsworks\_command\)<a name="data-bag-json-command"></a>

Represents settings for a command that AWS OpsWorks Stacks runs on one or more instances\.

The following example shows how to use Chef search to search through a single data bag item and then multiple data bag items to write messages to the Chef log with the commands' types and when they were sent:

```
command = search("aws_opsworks_command").first
Chef::Log.info("********** The command's type is '#{command['type']}' **********")
Chef::Log.info("********** The command was sent at '#{command['sent_at']}' **********")

search("aws_opsworks_command").each do |command|
  Chef::Log.info("********** The command's type is '#{command['type']}' **********")
  Chef::Log.info("********** The command was sent at '#{command['sent_at']}' **********")
end
```


****  

|  |  |  | 
| --- |--- |--- |
| [args](#data-bag-json-command-args) | [command\_id](#data-bag-json-command-command-id) | [iam\_user\_arn](#data-bag-json-command-iam-user-arn) | 
| [instance\_id](#data-bag-json-command-instance-id) | [sent\_at](#data-bag-json-command-sent-at) | [type](#data-bag-json-command-type) | 

**args**  <a name="data-bag-json-command-args"></a>
Arguments for the command \(string\)\.

**command\_id**  <a name="data-bag-json-command-command-id"></a>
The command's random unique identifier, assigned by AWS OpsWorks Stacks \(string\)\.

**iam\_user\_arn**  <a name="data-bag-json-command-iam-user-arn"></a>
If the command is created by the customer, the Amazon Resource Name \(ARN\) of the user who created the command \(string\)\.

**instance\_id**  <a name="data-bag-json-command-instance-id"></a>
The identifier of the instance that the command was run on \(string\)\.

**sent\_at**  <a name="data-bag-json-command-sent-at"></a>
The timestamp of when AWS OpsWorks Stacks ran the command \(string\)\.

**type**  <a name="data-bag-json-command-type"></a>
The command's type \(string\)\. Valid values include:  
+ `"configure"`
+ `"deploy"`
+ `"deregister"`
+ `"execute_recipes"`
+ `"grant_remote_access"`
+ `"install_dependencies"`
+ `"restart"`
+ `"revoke_remote_access"`
+ `"rollback"`
+ `"setup"`
+ `"shutdown"`
+ `"start"`
+ `"stop"`
+ `"sync_remote_users"`
+ `"undeploy"`
+ `"update_agent"`
+ `"update_custom_cookbooks"`
+ `"update_dependencies"`