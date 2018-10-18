# Using Search on a Windows Stack<a name="cookbooks-101-opsworks-opsworks-stack-config-search-windows"></a>

AWS OpsWorks Stacks provides two options to use search on Windows stacks\.
+ The `node` search index, which can be used to query a set of standard Chef attributes\.

  If you have existing recipes with search code that uses `node`, they will usually work on AWS OpsWorks Stacks stacks without modification\.
+ An additional set of search indexes that can be used to query sets of AWS OpsWorks Stacks\-specific attributes, and some standard attributes\.

  These indexes are discussed in [Using AWS OpsWorks Stacks\-Specific Search Indexes on Windows Stacks](cookbooks-101-opsworks-opsworks-stack-config-search-opsworks.md)\.

We recommend using `node` for retrieving standard information, such as hostnames or IP addresses\. That approach will keep your recipes consistent with standard Chef practice\. Use the AWS OpsWorks Stacks search indexes to retrieve information that is specific to AWS OpsWorks Stacks\.

**Topics**
+ [Using the node Search Index on Windows Stacks](cookbooks-101-opsworks-opsworks-stack-config-search-node.md)
+ [Using AWS OpsWorks Stacks\-Specific Search Indexes on Windows Stacks](cookbooks-101-opsworks-opsworks-stack-config-search-opsworks.md)