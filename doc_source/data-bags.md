# AWS OpsWorks Stacks Data Bag Reference<a name="data-bags"></a>

AWS OpsWorks Stacks exposes a wide variety of settings to recipes as Chef data bag content\. This reference lists this data bag content\.

A *data bag* is a Chef concept\. A data bag is a global variable that is stored as JSON data on an instance; the JSON data is accessible from Chef\. For example, a data bag can store global variables such as an app's source URL, the instance's hostname, and the associated stack's VPC identifier\. AWS OpsWorks Stacks stores its data bags on each stack's instances\. On Linux instances, AWS OpsWorks Stacks stores data bags in the `/var/chef/runs/run-ID/data_bags` directory\. On Windows instances, it stores data bags in the `drive:\chef\runs\run-id\data_bags` directory\. In both cases, *run\-ID* is a unique ID that AWS OpsWorks Stacks assigns to each Chef run on an instance\. These directories include a set of data bags \(subdirectories\)\. Each data bag contains zero or more data bag items, which are JSON\-formatted files that contain sets of data bag content\.

**Note**  
AWS OpsWorks Stacks does not support encrypted data bags\. To store sensitive data in encrypted form, such as passwords or certificates, we recommend storing it in a private S3 bucket\. You can then create a custom recipe that uses the [Amazon SDK for Ruby](http://aws.amazon.com/documentation/sdk-for-ruby/) to retrieve the data\. For an example, see [Using the SDK for Ruby](cookbooks-101-opsworks-s3.md)\.

Data bag content can include any of the following:
+ **String** content that follows standard Ruby syntax and can use single or double quotes, although strings containing certain special characters must have double quotes\. For more information, go to the [Ruby](http://www.ruby-lang.org/en/documentation/) documentation site\.
+ **Boolean** content, which is either `true` or `false` \(no quotes\)\.
+ **Number** content, which is either integer or decimal numbers, such as `4` or `2.5` \(no quotes\)\.
+ **List** content, which takes the form of comma\-separated values enclosed in square brackets \(no quotes\), such as `[ '80', '443' ]`
+ **JSON objects**, which contain additional data bag content, such as `"my-app": {"elastic_ip": null,...}`\.

Chef recipes can access data bags, data bag items, and data bag content through Chef search or directly\. The following describes how to use both access approaches \(although Chef search is preferred\)\.

To access a data bag through Chef search, use the [search](https://docs.chef.io/dsl_recipe.html#search) method, specifying the desired search index\. AWS OpsWorks Stacks provides the following search indexes:
+ [aws\_opsworks\_app](data-bag-json-app.md), which represents a set of deployed apps for a stack\.
+ [aws\_opsworks\_command](data-bag-json-command.md), which represents a set of commands that were run on a stack\. 
+ [aws\_opsworks\_ecs\_cluster](data-bag-json-ecs-cluster.md), which represents a set of Amazon Elastic Container Service \(Amazon ECS\) cluster instances for a stack\. 
+ [aws\_opsworks\_elastic\_load\_balancer](data-bag-json-elb.md), which represents a set of Elastic Load Balancing load balancers for a stack\.
+ [aws\_opsworks\_instance](data-bag-json-instance.md), which represents a set of instances for a stack\.
+ [aws\_opsworks\_layer](data-bag-json-layer.md), which represents a set of layers for a stack\.
+ [aws\_opsworks\_rds\_db\_instance](data-bag-json-rds.md), which represents a set of Amazon Relational Database Service \(Amazon RDS\) instances for a stack\.
+ [aws\_opsworks\_stack](data-bag-json-stack.md), which represents a stack\.
+ [aws\_opsworks\_user](data-bag-json-user.md), which represents a set of users for a stack\.

Once you know the search index name, you can access the content of the data bag for that search index\. For example, the following recipe code uses the `aws_opsworks_app` search index to get the content of the first data bag item \(the first JSON file\) in the`aws_opsworks_app` data bag \(the `aws_opsworks_app` directory\)\. The code then writes two messages to the Chef log, one with the app's shortname data bag content \(a string in the JSON file\), and another with the app's source URL data bag content \(another string in the JSON file\):

```
app = search("aws_opsworks_app").first
Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
```

Where `['shortname']` and `['app_source']['url']` specify the following data bag content in the corresponding JSON file:

```
{
  ...
  "shortname": "mylinuxdemoapp",
  ...
  "app_source": {
    ...
    "url": "https://s3.amazonaws.com/opsworks-chef-12-linux-demo/opsworks-linux-demo-nodejs.tar.gz",
  },
  ...  
}
```

For a list of the data bag content that you can search for, see the reference topics in this section\.

You can also iterate through a set of data bag items in a data bag\. For example, the following recipe code is similar to the previous example; it iterates through each of the data bag items in the data bag when there is more than one data bag item:

```
search("aws_opsworks_app").each do |app|
  Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
  Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
end
```

If you know that specific data bag content exists, you can find the corresponding data bag item with the following syntax:

```
search("search_index", "key:value").first
```

For example, the following recipe code uses the `aws_opsworks_app` search index to find the data bag item that contains the app short name of `mylinuxdemoapp`\. It then uses the data bag item's contents to write a message to the Chef log with the corresponding app's short name and source URL:

```
app = search("aws_opsworks_app", "shortname:mylinuxdemoapp").first
Chef::Log.info("********** For the app with the short name '#{app['shortname']}', the app's URL is '#{app['app_source']['url']}' **********")
```

For the `aws_opsworks_instance` search index only, you can specify `self:true` to represent the instance that the recipe is being executed on\. The following recipe code uses the corresponding data bag item's contents to write a message to the Chef log with the corresponding instance's AWS OpsWorks Stacks\-generated ID and operating system:

```
instance = search("aws_opsworks_instance", "self:true").first
Chef::Log.info("********** For instance '#{instance['instance_id']}', the instance's operating system is '#{instance['os']}' **********")
```

Instead of using Chef search to access data bags, data bag items, and data bag content, you can access them directly\. To do this, use the [data\_bag](https://docs.chef.io/dsl_recipe.html#data-bag) and [data\_bag\_item](https://docs.chef.io/dsl_recipe.html#data-bag-item) methods to access data bags and data bag items, respectively\. For example, the following recipe code does the same things as the previous examples, except that it directly accesses a single data bag item and then multiple data bag items when there are more than one:

```
# Syntax: data_bag_item("the data bag name", "the file name in the data bag without the file extension")
app = data_bag_item("aws_opsworks_app", "mylinuxdemoapp")
Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
    
data_bag("aws_opsworks_app").each do |data_bag_item|
  app = data_bag_item("aws_opsworks_app", data_bag_item)
  Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
  Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
end
```

Of these two approaches, we recommend that you use Chef search\. All related examples in this guide demonstrate this approach\.

**Topics**
+ [App Data Bag \(aws\_opsworks\_app\)](data-bag-json-app.md)
+ [Command Data Bag \(aws\_opsworks\_command\)](data-bag-json-command.md)
+ [Amazon ECS Cluster Data Bag \(aws\_opsworks\_ecs\_cluster\)](data-bag-json-ecs-cluster.md)
+ [Elastic Load Balancing Data Bag \(aws\_opsworks\_elastic\_load\_balancer\)](data-bag-json-elb.md)
+ [Instance Data Bag \(aws\_opsworks\_instance\)](data-bag-json-instance.md)
+ [Layer Data Bag \(aws\_opsworks\_layer\)](data-bag-json-layer.md)
+ [Amazon RDS Data Bag \(aws\_opsworks\_rds\_db\_instance\)](data-bag-json-rds.md)
+ [Stack Data Bag \(aws\_opsworks\_stack\)](data-bag-json-stack.md)
+ [User Data Bag \(aws\_opsworks\_user\)](data-bag-json-user.md)