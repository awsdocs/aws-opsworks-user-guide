# Rails App Server AWS OpsWorks Stacks Layer<a name="workinglayers-rails"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The Rails App Server layer is an AWS OpsWorks Stacks layer that provides a blueprint for instances that function as Rails application servers\.

**Installation**: AWS OpsWorks Stacks uses the instance's package installer to install the server packages in their default locations\. For more information about Apache/Passenger installation, see [Phusion Passenger](https://www.phusionpassenger.com/)\. For more information about logging, see [Log Files](http://httpd.apache.org/docs/2.2/logs.html)\. For more information about Nginx/Unicorn installation, see [Unicorn](http://unicorn.bogomips.org/)\.

The **Add Layer** page provides the following configuration options, all of which are optional\.

**Ruby Version**  
The Ruby version that will be used by your applications\. The default value is 2\.3\.  
You can also specify your preferred Ruby version by [overriding the `[:opsworks][:ruby_version]`](workingcookbook-attributes.md) attribute\.  
AWS OpsWorks Stacks installs a separate Ruby package to be used by recipes and the instance agent\. For more information, see [Ruby Versions](workingcookbook-ruby.md)\.

**Rails Stack**  
The default Rails stack is [Apache2](http://httpd.apache.org/) with [Phusion Passenger](https://www.phusionpassenger.com/)\. You can also use [Nginx](http://nginx.org/en/) with [Unicorn](http://unicorn.bogomips.org/)\.  
If you use Nginx and Unicorn, you must add the unicorn gem to your app's Gemfile, as in the following example:  

```
source 'https://rubygems.org'
gem 'rails', '3.2.15'
...
# Use unicorn as the app server
gem 'unicorn'
...
```

**Passenger Version**  
If you have specified Apache2/Passenger, you must specify the Passenger version\. The default value is 5\.0\.28\.

**Rubygems Version**  
The default [Rubygems](http://rubygems.org/) version is 2\.5\.1

**Install and Manage Bundler**  
Lets you choose whether to install and manage [Bundler](http://gembundler.com/)\. The default value is **Yes**\.

**Bundler version**  
The default Bundler version is 1\.12\.5\.

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Elastic Load Balancer**  
You can attach an Elastic Load Balancing load balancer to the layer's instances\.

You can modify some configuration settings by using custom JSON or a custom attributes file\. For more information, see [Overriding Attributes](workingcookbook-attributes.md)\. For a list of Apache, Nginx, Phusion Passenger, and Unicorn attributes that can be overridden, see [Built\-in Cookbook Attributes](attributes-recipes.md)\.

**Important**  
If your Ruby on Rails application uses SSL, we recommend that you disable SSLv3 if possible to address the vulnerabilities described in [CVE\-2014\-3566](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566)\. For more information, see [Disabling SSLv3 for Rails Servers](#workinglayers-rails-sslv3)\.

**Topics**
+ [Disabling SSLv3 for Rails Servers](#workinglayers-rails-sslv3)
+ [Connecting to a Database](#workinglayers-rails-db)
+ [Deploying Ruby on Rails Apps](#workinglayers-rails-deploy)

## Disabling SSLv3 for Rails Servers<a name="workinglayers-rails-sslv3"></a>

To disable SSLv3 for Rails servers, update the layer's **Ruby Version** setting to 2\.1 or higher, which installs Ruby 2\.1\.4 or higher as the version that applications use\.
+ Update the layer's **Ruby Version** setting to 2\.1 or higher\.
+ Update the configuration file for your Rails stack, as follows\.

**Apache with Phusion Passenger**  
Update `SSLProtocol` setting in the Apache server's `ssl.conf` file, as described in [Disabling SSLv3 for Apache Servers](layers-java.md#layers-java-sslv3)\.

**Nginx with Unicorn**  
Add an explicit `ssl_protocols` directive to the Nginx server's `nginx.conf` file\. To disable SSLv3, override the built\-in [nginx cookbook's](https://github.com/aws/opsworks-cookbooks/tree/release-chef-11.10/nginx) `nginx.conf.erb` template file, which the Rails App Server layer's Setup recipes use to create `nginx.conf`, and add the following directive:  

```
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
```
For more information on how to configure `nginx.conf`, see [Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html)\. For more information on how to override a built\-in template, see [Using Custom Templates](workingcookbook-template-override.md)\.

## Connecting to a Database<a name="workinglayers-rails-db"></a>

When you deploy an app, AWS OpsWorks Stacks creates a new `database.yml` file using information from the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy)\. If you [attach a MySQL or Amazon RDS instance](workingapps-creating.md#workingapps-creating-data) to the app, AWS OpsWorks Stacks adds the connection information to the `deploy` attributes, so that `database.yml` automatically contains the correct connection data\. 

If an app does not have an attached database, by default, AWS OpsWorks Stacks does not add any connection information to the `deploy` attributes and does not create `database.yml`\. If you want to use a different database, you can use custom JSON to add database attributes to the app's `deploy` attributes with the connection information\. The attributes are all under`["deploy"]["appshortname"]["database"]`, where *appshortname* is the app's short name, which AWS OpsWorks Stacks generates from the app name\. The values you specify in custom JSON override any default settings\. For more information, see [Adding Apps](workingapps-creating.md)\.

AWS OpsWorks Stacks incorporates the following [`[:...][:database]`](attributes-json-deploy.md#attributes-json-deploy-app-db) attribute values into `database.yml`\. The required attributes depend on the particular database, but you must have a `host` attribute or AWS OpsWorks Stacks will not create `database.yml`\.
+ `[:adapter] (String)` – The database adapter, such as `mysql`\.
+ `[:database]` \(String\) – The database name\.
+ `[:encoding]` \(String\) – The encoding, which is typically set to `utf8`\.
+ `[:host]` \(String\) – The host URL, such as `railsexample.cdlqlk5uwd0k.us-west-2.rds.amazonaws.com`\.
+ `[:reconnect]` \(Boolean\) – Whether the application should reconnect if the connection no longer exists\.
+ `[:password]` \(String\) – The database password\.
+ `[:port]` \(Number\)\. – The database's port number\. Use this attribute to override the default port number, which is set by is set by the adapter\.
+ `[:username]` \(String\) – The database user name\.

The following example shows custom JSON for an app whose short name is *myapp*\.

```
{
  "deploy" : {
    "myapp" : {
      "database" : {
        "adapter" : "adapter",
        "database" : "databasename",
        "host" : "host",
        "password" : "password",
        "port" : portnumber
        "reconnect" : true/false,
        "username" : "username"
      }
    }
  }
}
```

For information on how to specify custom JSON, see [Using Custom JSON](workingstacks-json.md)\. To see the template used to create `database.yml` \(`database.yml.erb`\), go to the [built\-in cookbook repository](https://github.com/aws/opsworks-cookbooks/tree/release-chef-11.4/rails/templates/default)\.

## Deploying Ruby on Rails Apps<a name="workinglayers-rails-deploy"></a>

You can deploy Ruby on Rails apps from any of the supported repositories\. The following shows how to deploy an example Ruby on Rails app to a server running an Apache/Passenger Rails stack\. The example code is stored in a public GitHub repository, but the basic procedure is the same for the other supported repositories\. For more information on how to create and deploy apps, see [Apps](workingapps.md)\. To view the example's code, which includes extensive comments, go to [https://github\.com/awslabs/opsworks\-demo\-rails\-photo\-share\-app](https://github.com/awslabs/opsworks-demo-rails-photo-share-app)\. 

**To deploy a Ruby on Rails app from a GitHub repository**

1. [Create a stack](workingstacks-creating.md) with a Rails App Server layer with Apache/Passenger as the Rails stack, [add a 24/7 instance](workinginstances-add.md) to the layer, and [start it](workinginstances-starting.md)\. 

1. After the instance is online, [add an app](workingapps-creating.md#workingapps-creating-general) to the stack and specify following settings:
   + **Name** – Any name you prefer; the example uses `PhotoPoll`\.

     AWS OpsWorks Stacks uses this name for display purposes, and generates a short name for internal use and to identify the app in the [stack configuration and deployment attributes](workingcookbook-json.md)\. For example, the PhotoPoll short name is photopoll\.
   + **App type** – **Ruby on Rails**\.
   + ** Rails environment** – The available environments are determined by the application\.

     The example app has three: **development**, **test**, and **production**\. For this example, set the environment to **development**\. See the example code for descriptions of each environment\.
   + **Repository type** – Any of the supported repository types\. Specify `Git` for this example
   + **Repository URL** – The repository that the code should be deployed from\.

     For this example, set the URL to **git://github\.com/awslabs/opsworks\-demo\-rails\-photo\-share\-app**\.

   Use the default values for the remaining settings and then click **Add App** to create the app\.

1. [Deploy the app](workingapps-deploying.md) to the Rails App Server instance\.

1. When deployment is finished, go to the **Instances** page and click the Rails App Server instance's public IP address\. You should see the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/rails_example.png)