# Other Top\-Level Attributes<a name="attributes-json-other"></a>

This section contains top\-level stack configuration attributes that do not have child attributes\.

**rails Attributes**  
Contains a **max\_pool\_size** attribute that specifies the server's maximum pool size \(number\)\. The attribute value is set by AWS OpsWorks Stacks and depends on the instance type, but you can override it by using custom JSON or a custom attribute file\.   

```
node["rails"]["max_pool_size"]
```

**recipes Attributes**  
A list of the built\-in recipes that were run by this activity, using the `"cookbookname::recipename"` format \(list of string\)\.  

```
node["recipes"]
```

**opsworks\_rubygems Attributes**  
Contains a **version** element that specifies the RubyGems version \(string\)\.  

```
node["opsworks_rubygems"]["version"]
```

**languages Attributes**  
Contains an attribute for each installed language, named for the language, such as **ruby**\. The attribute is an object that contains an attribute, such as **ruby\_bin**, that specifies the installation folder, such as `"/usr/bin/ruby"` \(string\)\.

**ssh\_users Attributes**  
Contains a set of attributes, each of which describes one of the IAM users that have been granted SSH permissions\. Each attribute is named with a user's Unix ID\. AWS OpsWorks Stacks generates a unique ID for each user in the 2000 range, such as `"2001"`, and creates a user with that ID on every instance\. Each attribute contains the following attributes:    
**email**  
The IAM user's e\-mail address \(string\)\.  

```
node["ssh_users"]["UnixID"]["email"]
```  
**public\_key**  
The IAM user's public SSH key \(string\)\.  

```
node["ssh_users"]["UnixID"]["public_key"]
```  
**sudoer**  
Whether the IAM user has sudo permissions \(Boolean\)\.  

```
node["ssh_users"]["UnixID"]["sudoer"]
```  
**name**  
The IAM user name \(string\)\.  

```
node["ssh_users"]["UnixID"]["name"]
```