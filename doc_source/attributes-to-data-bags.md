# Moving Stack Settings from Attributes to Data Bags<a name="attributes-to-data-bags"></a>

AWS OpsWorks Stacks exposes a wide variety of stack settings to your Chef recipes\. These stack settings include values such as:
+ Stack cookbook source URLs
+ Layer volume configurations
+ Instance host names
+ Elastic Load Balancing DNS names
+ App source URLs
+ User names

Referencing stack settings from recipes makes recipe code more robust and less error prone than hard\-coding stack settings directly in recipes\. This topic describes how to access these stack settings as well as how to move from attributes in Chef 11\.10 and earlier versions for Linux to data bags in Chef 12 Linux\.

In Chef 11\.10 and earlier versions for Linux, stack settings are available as [Chef attributes](https://docs.chef.io/attributes.html) and are accessed through the Chef `node` object or through Chef search\. These attributes are stored on AWS OpsWorks Stacks instances in a set of JSON files in the `/var/lib/aws/opsworks/chef` directory\. For more information, see [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md)\.

In Chef 12 Linux, stack settings are available as [Chef data bags](https://docs.chef.io/data_bags.html) and are accessed only through Chef search\. Data bags are stored on AWS OpsWorks Stacks instances in a set of JSON files in the `/var/chef/runs/run-ID/data_bags` directory, where *run\-ID* is a unique ID that AWS OpsWorks Stacks assigns to each Chef run on an instance\. Stack settings are no longer available as Chef attributes, so stack settings can no longer be accessed through the Chef `node` object\. For more information, see the [AWS OpsWorks Stacks Data Bag Reference](data-bags.md)\.

For example, in Chef 11\.10 and earlier versions for Linux, the following recipe code uses the Chef `node` object to get attributes representing an app's short name and source URL\. It then uses the Chef log to write these two attribute values:

```
Chef::Log.info ("********** The app's short name is '#{node['opsworks']['applications'].first['slug_name']}' **********")
Chef::Log.info("********** The app's URL is '#{node['deploy']['simplephpapp']['scm']['repository']}' **********")
```

In Chef 12 Linux, the following recipe code uses the `aws_opsworks_app` search index to get the contents of the first data bag item in the `aws_opsworks_app` data bag\. The code then writes two messages to the Chef log, one with the app's short name data bag content, and another with the app's source URL data bag content:

```
app = search("aws_opsworks_app").first

Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
```

To migrate your recipe code that accesses stack settings from Chef 11\.10 and earlier versions for Linux to Chef 12 Linux, you must revise your code to:
+ Access Chef data bags instead of Chef attributes\.
+ Use Chef search instead of the Chef `node` object\.
+ Use AWS OpsWorks Stacks data bag names such as `aws_opsworks_app`, instead of using AWS OpsWorks Stacks attribute names such as `opsworks` and `deploy`\.

For more information, see the [AWS OpsWorks Stacks Data Bag Reference](data-bags.md)\.