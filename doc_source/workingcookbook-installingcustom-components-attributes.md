# Attributes<a name="workingcookbook-installingcustom-components-attributes"></a>

Recipes and templates depend on a variety of values, such as configuration settings\. Rather than hardcode such values directly in recipes or templates, you can create an attribute file with an attribute that represents each value\. You then use the attributes in your recipes or templates instead of explicit values\. The advantage of using attributes is that you can override their values without touching the cookbook\. For this reason, you should always use attributes to define the following types of values:
+ Values that might vary from stack to stack or with time, such as user names\.

  If you hardcode such values, you must change the recipe or template each time you need to change a value\. By using attributes to define these values, you can use the same cookbooks for every stack and just override the appropriate attributes\.
+ Sensitive values, such as passwords or secret keys\.

  Putting explicit sensitive values in your cookbook can increase the risk of exposure\. Instead, define attributes with dummy values and override them to set the actual values\. The best way to override such attributes is with custom JSON\. For more information, see [Using Custom JSON](workingcookbook-json-override.md)\.

For more information about attributes and how to override them, see [Overriding Attributes](workingcookbook-attributes.md)\. 

The following example is a portion of an example attribute file\.

```
...
default["apache"]["listen_ports"] = [ '80','443' ]
default["apache"]["contact"] = 'ops@example.com'
default["apache"]["timeout"] = 120
default["apache"]["keepalive"] = 'Off'
default["apache"]["keepaliverequests"] = 100
default["apache"]["keepalivetimeout"] = 3
default["apache"]["prefork"]["startservers"] = 16
default["apache"]["prefork"]["minspareservers"] = 16
default["apache"]["prefork"]["maxspareservers"] = 32
default["apache"]["prefork"]["serverlimit"] = 400
default["apache"]["prefork"]["maxclients"] = 400
default["apache"]["prefork"]["maxrequestsperchild"] = 10000
...
```

 AWS OpsWorks Stacks defines attributes by using the following syntax:

```
node.type["attribute"]["subattribute"]["..."]=value
```

You can also use colons \(:\), as follows:

```
node.type[:attribute][:subattribute][:...]=value
```

An attribute definition has the following components:

## `node.`<a name="node"></a>

The `node.` prefix is optional and usually omitted, as shown in the example\.

## `type`<a name="type"></a>

The type governs whether the attribute can be overridden\. AWS OpsWorks Stacks attributes typically use one of the following types:
+ `default` is the most commonly used type, because it allows the attribute to be overridden\.
+ `normal` defines an attribute that overrides one of the standard AWS OpsWorks Stacks attribute values\.

**Note**  
Chef supports additional types, which aren't necessary for AWS OpsWorks Stacks but might be useful for your project\. For more information, see [About Attributes](http://docs.chef.io/attributes.html)\.

## `attribute name`<a name="attribute-name"></a>

The attribute name uses the standard Chef node syntax, `[:attribute][:subattribute][...]`\. You can use any names you like for your attributes\. However, as discussed in [Overriding Attributes](workingcookbook-attributes.md), custom cookbook attributes are merged into the instance's node object, along with the attributes from the stack configuration and deployment attributes, and Chef's [Ohai tool](https://docs.chef.io/ohai.html)\. Commonly used configuration names such as *port* or *user* might appear in a variety of cookbooks\.

To avoid name collisions, the convention is to create qualified attribute names with at least two elements, as shown in the example\. The first element should be unique and is typically based on a product name like *Apache*\. It is followed by one or more subattributes that identify the particular value, such as `[:user]` or `[:port]`\. You can use as many subattributes as are appropriate for your project\.

## `value`<a name="value"></a>

An attribute can be set to the following types of values:
+ A string, such as `default[:apache][:keepalive] = 'Off'`\.
+ A number \(without quotes\) such as `default[:apache][:timeout] = 120`\.
+ A Boolean value, which can be either `true` or `false` \(no quotes\)\.
+ A list of values, such as `default[:apache][:listen_ports] = [ '80','443' ]`

The attribute file is a Ruby application, so you can also use node syntax and logical operators to assign values based on other attributes\. For more information about how to define attributes, see [About Attributes s](https://docs.chef.io/chef_overview_attributes.html)\. For examples of working attribute files, see the AWS OpsWorks Stacks built\-in cookbooks at [https://github\.com/aws/opsworks\-cookbooks](https://github.com/aws/opsworks-cookbooks)\.