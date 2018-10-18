# Overriding Built\-In Attributes<a name="cookbooks-101-opsworks-attributes"></a>

**Note**  
This topic applies only to Linux stacks\. You cannot override built\-in attributes on Windows stacks\.

AWS OpsWorks Stacks installs a set of built\-in cookbooks on each instance\. Many of the built\-in cookbooks support the built\-in layers, and their attribute files define a variety of default system and application settings, such as the Apache server configuration settings\. By putting these settings in attribute files, you can customize many configuration settings by overriding the corresponding built\-in attribute in either of the following ways:
+ Define the attribute in custom JSON\.

  This approach has the advantage of being simple and flexible\. However, you must enter custom JSON manually, so there is no robust way to manage the attribute definitions\.
+ Implement a custom cookbook and define the attribute in a `customize.rb` attribute file\.

  This approach is less flexible than using custom JSON, but is more robust because you can put custom cookbooks under source control\.

This topic describes how to use a custom cookbook attribute file to override built\-in attributes, using the Apache server as an example\. For more information on how to override attributes with custom JSON, see [Using Custom JSON](workingcookbook-json-override.md)\. For a general discussion of how to override attributes, see [Overriding Attributes](workingcookbook-attributes.md)\.

**Note**  
Overriding attributes is the preferred way to customize configuration settings, but settings are not always represented by attributes\. In that case, you can often customize the configuration file by overriding the template that the built\-in recipes use to create the configuration file\. For an example, see [Overriding Built\-In Templates](cookbooks-101-opsworks-templates.md)\.

The built\-in attributes typically represent values in the template files that Setup recipes use to create configuration files\. For example, one of the `apache2` Setup recipes, [https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/recipes/default.rb](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/recipes/default.rb), uses the [https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/templates/default/apache2.conf.erb](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/templates/default/apache2.conf.erb) template to create the Apache server's main configuration file, `httpd.conf` \(Amazon Linux\) or `apache2.conf` \(Ubuntu\)\. The following is an excerpt from the template file:

```
...
#
# MaxKeepAliveRequests: The maximum number of requests to allow
# during a persistent connection. Set to 0 to allow an unlimited amount.
# We recommend you leave this number high, for maximum performance.
#
MaxKeepAliveRequests <%= node[:apache][:keepaliverequests] %>
#
# KeepAliveTimeout: Number of seconds to wait for the next request from the
# same client on the same connection.
#
KeepAliveTimeout <%= node[:apache][:keepalivetimeout] %>
##
## Server-Pool Size Regulation (MPM specific)
##

...
```

The `KeepAliveTimeout` setting in this example is the value of the `[:apache][:keepalivetimeout]` attribute\. This attribute's default value is defined in the `apache2` cookbook's [https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/attributes/apache.rb](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/attributes/apache.rb) attribute file, as shown in the following excerpt:

```
...
# General settings
default[:apache][:listen_ports] = [ '80','443' ]
default[:apache][:contact] = 'ops@example.com'
default[:apache][:log_level] = 'info'
default[:apache][:timeout] = 120
default[:apache][:keepalive] = 'Off'
default[:apache][:keepaliverequests] = 100
default[:apache][:keepalivetimeout] = 3
...
```

**Note**  
For more information about commonly used built\-in attributes, see [Built\-in Cookbook Attributes](attributes-recipes.md)\.

To support overriding built\-in attributes, all built\-in cookbooks contain a `customize.rb` attribute file, which is incorporated into all modules through an `include_attribute` directive\. The built\-in cookbooks' `customize.rb` files contain no attribute definitions and have no effect on the built\-in attributes\. To override the built\-in attributes, you create a custom cookbook with the same name as the built\-in cookbook and put your custom attribute definitions in an attribute file that is also named `customize.rb`\. That file takes precedence over the built\-in version, and is included in any related modules\. If you define any built\-in attributes in your `customize.rb`, they override the corresponding built\-in attributes\.

This example shows how to override the built\-in `[:apache][:keepalivetimeout]` attribute to set its value to 5 instead of 3\. You can use a similar approach for any built\-in attribute\. However, be careful which attributes you override\. For example, overriding attributes in the `opsworks` namespace might cause problems for some built\-in recipes\. 

**Important**  
Do not override built\-in attributes by modifying a copy of the built\-in attributes file itself\. For example, you *could* put a copy of `apache.rb` in your custom cookbook's `apache2/attributes` folder and modify some of its settings\. However, this file takes precedence over the built\-in version, and the built\-in recipes will now use your version of `apache.rb`\. If AWS OpsWorks Stacks later modifies the built\-in `apache.rb` file, recipes will not get the new values unless you manually update your version\. By using `customize.rb`, you override only the specified attributes; the built\-in recipes continue to automatically get up\-to\-date values for every attribute that you have not overridden\.

To start, create a custom cookbook\.

**To create the cookbook**

1. Within your `opsworks_cookbooks` directory, create a cookbook directory named `apache2` and navigate to it\.

   To override built\-in attributes, the custom cookbook must have the same name as the built\-in cookbook, `apache2` for this example\.

1. In the `apache2` directory, create an `attributes` directory\.

1. Add a file named `customize.rb` to the `attributes` directory and use it to define the built\-in cookbook attributes that you want to override\. For this example, the file should contain the following: 

   ```
   normal[:apache][:keepalivetimeout] = 5
   ```
**Important**  
To override a built\-in attribute, a custom attribute must be a `normal` type or higher and have exactly the same node name as the corresponding built\-in attribute\. The `normal` type ensures that the custom attribute takes precedence over the built\-in attributes, which are all `default` type\. For more information, see [Attribute Precedence](workingcookbook-attributes-precedence.md)\.

1. Create a `.zip` archive of `opsworks_cookbooks` named `opsworks_cookbooks.zip` and upload the archive to an Amazon Simple Storage Service \(Amazon S3\) bucket\. For simplicity, [make the file public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html)\. Record the URL for later use\. You can also store your cookbooks in a private Amazon S3 archive or in other repository types\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

To use the custom attribute, create a stack and install the cookbook\.

**To use the custom attribute**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/), and then choose **Add Stack**\.

1. Specify the following standard settings\.
   + **Name** – ApacheConfig
   + **Region** – US West \(Oregon\)

     You can put your stack in any region, but we recommend US West \(Oregon\) for tutorials\.
   + **Default SSH key** – An EC2 key pair

     If you need to create an EC2 key pair, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. Note that the key pair must belong to the same AWS region as the stack\.

   Choose **Advanced>>**, set **Use custom Chef cookbooks** to **Yes**, and then specify the following settings\.
   + **Repository type** – **Http Archive**
   + **Repository URL** – The cookbook archive's URL that you recorded earlier

   Accept the defaults for the other settings, and then choose **Add Stack** to create the stack\.
**Note**  
This example uses the default operating system, Amazon Linux\. You can use Ubuntu, if you prefer\. The only difference is that on Ubuntu systems, the built\-in Setup recipe produces a configuration file with the same settings named `apache2.conf` and puts it in the `/etc/apache2` directory\. 

1. Choose **Add a layer**, and then [add a Java App Server layer](layers-java.md) with default settings to the stack\.

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the layer, and then start the instance\.

   A t2\.micro instance is sufficient for this example\.

1. After the instance is online, [connect to it with SSH](workinginstances-ssh.md)\. The `httpd.conf` file is in the `/etc/httpd/conf` directory\. If you examine the file, you should see your custom `KeepAliveTimeout` setting\. The remainder of the settings will have the default values from the built\-in `apache.rb` file\. The relevant part of `httpd.conf` should look similar to the following:

   ```
   ...
   #
   # KeepAliveTimeout: Number of seconds to wait for the next request from the
   # same client on the same connection.
   #
   KeepAliveTimeout 5
   ...
   ```