# Obtaining Attribute Values with Chef Search<a name="cookbooks-101-opsworks-opsworks-stack-config-search"></a>

**Note**  
This approach is available for Windows stacks and Chef 11\.10 Linux stacks\.

Obtaining stack configuration and deployment attribute values directly from the node object can be complicated, and can't be used with Windows stacks\. An alternative approach is to use [Chef search](http://docs.chef.io/chef_search.html) to query for the attributes of interest\. If you are familiar with Chef server, you will find that Chef search works a bit differently with AWS OpsWorks Stacks\. Because AWS OpsWorks Stacks uses chef\-client in local mode, Chef search depends on a local version of Chef server called chef\-zero, so that search operates on the data that is stored locally in the instance's node object instead of on a remote server\.

As a practical matter, restricting search to locally stored data usually doesn't matter because the node object on an AWS OpsWorks Stacks instance includes the [stack configuration and deployment attributes](workingcookbook-json.md)\. They contain most if not all of the data that recipes would typically obtain from Chef server and use the same names, so you can usually use search code written for Chef server on AWS OpsWorks Stacks instances without modification\. For more information, see [Using Chef Search](workingcookbook-chef11-10.md#workingcookbook-chef11-10-search)\.

The following shows the basic structure of a search query:

```
result = search(:search_index, "key:pattern")
```
+ The search index specifies what attributes the query applies to and determines the type of object that is returned\.
+ The key specifies the attribute name\.
+ The pattern specifies which values of the attribute that you want to retrieve\.

  You can query for specific attribute values or use wild cards to query for a range of values\.
+ The result is a list of objects that satisfy the query, each of which is a hash table containing multiple related attributes\.

  For example, if you use the `node` search index, the query returns a list of instance objects, one for each instance that satisfies the query\. Each object is a hash table that contains a set of attributes that define the instance configuration, such as the hostname and IP address\.

For example, the following query uses the `node` search index, which is a standard Chef index that applies to the stack's instances \(or nodes, in Chef terminology\)\. It searches for instances with hostname of `myhost`\.

```
result = search(:node, "hostname:myhost")
```

Search returns a list of instance objects whose hostname is `myhost`\. If you want the first instance's operating system, for example, it would be represented by `result[0][:os]`\. If the query returns multiple objects, you can enumerate them to retrieve the required information\.

The details of how to use search in a recipe depend on whether you are using a Linux or Windows stack\. The following topics provide examples for both stack types\.

**Topics**
+ [Using Search on a Linux Stack](cookbooks-101-opsworks-opsworks-stack-config-search-linux.md)
+ [Using Search on a Windows Stack](cookbooks-101-opsworks-opsworks-stack-config-search-windows.md)