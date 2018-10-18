# Stack Configuration and Deployment Attributes: Linux<a name="attributes-json-linux"></a>

This topic includes the most commonly used stack configuration and deployment attributes and their associated node syntax\. It is organized around the stack configuration namespace structure that is used by Linux stacks\. Note that the same attribute names are sometimes used for different purposes, and occur in different namespaces\. For example, `id` can refer to a stack ID, a layer ID, an app ID, and so on, so you need the fully qualified name to use the attribute value\. A convenient way to visualize this data is as a JSON object\. For examples , see [Stack Configuration and Deployment Attributes](workingcookbook-json.md)\.

**Note**  
On Linux instances, AWS OpsWorks Stacks installs this JSON object on each instance in addition to adding the data to the node object\. You can retrieve it by using the [agent CLI's `get_json` command](agent-json.md)\.

**Topics**
+ [opsworks Attributes](attributes-json-opsworks.md)
+ [opsworks\_custom\_cookbooks Attributes](attributes-json-custom.md)
+ [dependencies Attributes](attributes-json-dependencies.md)
+ [ganglia Attributes](attributes-json-ganglia.md)
+ [mysql Attributes](attributes-json-mysql.md)
+ [passenger Attributes](attributes-json-passenger.md)
+ [opsworks\_bundler Attributes](attributes-json-bundler.md)
+ [deploy Attributes](attributes-json-deploy.md)
+ [Other Top\-Level Attributes](attributes-json-other.md)