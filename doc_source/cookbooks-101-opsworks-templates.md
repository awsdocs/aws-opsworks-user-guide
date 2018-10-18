# Overriding Built\-In Templates<a name="cookbooks-101-opsworks-templates"></a>

**Note**  
This topic applies only to Linux stacks\. You cannot override built\-in templates on Windows stacks\.

The AWS OpsWorks Stacks built\-in recipes use templates to create files on instances, primarily configuration files for servers, such as Apache\. For example, the `apache2` recipes use the [https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/templates/default/apache2.conf.erb](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/templates/default/apache2.conf.erb) template to create the Apache server's primary configuration file, `httpd.conf` \(Amazon Linux\) or `apache2.conf` \(Ubuntu\)\. 

Most of the configuration settings in these templates are represented by attributes, so the preferred way to customize a configuration file is by overriding the appropriate built\-in attributes\. For an example, see [Overriding Built\-In Attributes](cookbooks-101-opsworks-attributes.md)\. However, if the settings that you want to customize aren't represented by built\-in attributes, or aren't in the template at all, you must override the template itself\. This topic describes how to override a built\-in template to specify a custom Apache configuration setting\.

You can provide custom error responses to Apache by adding `ErrorDocument` settings to the `httpd.conf` file\. `apache2.conf.erb` contains only some commented\-out examples, as shown in the following:

```
...
#
# Customizable error responses come in three flavors:
# 1) plain text 2) local redirects 3) external redirects
#
# Some examples:
#ErrorDocument 500 "The server made a boo boo."
#ErrorDocument 404 /missing.html
#ErrorDocument 404 "/cgi-bin/missing_handler.pl"
#ErrorDocument 402 http://www.example.com/subscription_info.html
...
```

Because these settings are hardcoded comments, you can't specify custom values by overriding attributes; you must override the template itself\. However, unlike with attributes, there is no way to override particular parts of a template file\. You must create a custom cookbook with the same name as the built\-in version, copy the template file to the same subdirectory, and modify the file as needed\. This topic shows how to override `apache2.conf.erb` to provide a custom response to error 500\. For a general discussion of overriding templates, see [Using Custom Templates](workingcookbook-template-override.md)\.

**Important**  
When you override a buiIt\-in template, the built\-in recipes use your customized version of the template instead of the built\-in version\. If AWS OpsWorks Stacks updates the built\-in template, the custom template becomes out of sync and might not work correctly\. AWS OpsWorks Stacks doesn't make such changes often, and when a template does change, AWS OpsWorks Stacks lists the changes and gives you the option of upgrading to a new version\. We recommend that you monitor the [AWS OpsWorks Stacks repository](https://github.com/aws/opsworks-cookbooks) for changes, and manually update your custom template as needed\. Note that the repository has a separate branch for each supported Chef version, so be sure that you are in the correct branch\.

To start, create a custom cookbook\.

**To create the cookbook**

1. In the `opsworks_cookbooks` directory, create a cookbook directory named `apache2`, and then navigate to it\. To override built\-in templates, the custom cookbook must have the same name as the built\-in cookbook, `apache2` for this example\.
**Note**  
If you have already completed the [Overriding Built\-In Attributes](cookbooks-101-opsworks-attributes.md) walkthrough, you can use the same `apache2` cookbook for this example, and skip Step 2\.

1. Create a `metadata.rb` file with the following content, and then save it to the `apache2` directory\.

   ```
   name "apache2"
   version "0.1.0"
   ```

1. In `apache2` directory, create a `templates/default` directory\.\.
**Note**  
The `templates/default` directory works for Amazon Linux and Ubuntu 12\.04 instances, which use the default `apache2.conf.erb` template\. Ubuntu 14\.04 instances use an operating system\-specific `apache2.conf.erb` template, which is in the `templates/ubuntu-14.04` directory\. If you want the customization to apply to Ubuntu 14\.04 instances also, you must override that template too\.

1. Copy the [built\-in `apache2.conf.erb` template](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/templates/default/apache2.conf.erb) to your `templates/default` directory\. Open the template file, uncomment the `ErrorDocument 500` line, and provide a custom error message, as follows: 

   ```
   ...
   ErrorDocument 500 "A custom error message."
   #ErrorDocument 404 /missing.html
   ...
   ```

1. Create a `.zip` archive of `opsworks_cookbooks` named `opsworks_cookbooks.zip`, and then upload the file to an Amazon Simple Storage Service \(Amazon S3\) bucket\. For simplicity, [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html)\. Record the archive's URL for later use\. You can also store your cookbooks in a private Amazon S3 archive or in other repository types\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

   Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**Note**  
For simplicity, this example adds a hardcoded error message to the template\. To change it, you must modify the template and [reinstall the cookbook](workingcookbook-installingcustom-enable-update.md)\. To give yourself greater flexibility, you can [define a default custom attribute](cookbooks-101-opsworks-attributes.md) for the error string in the custom cookbook's `customize.rb` attribute file and assign the value of that attribute to `ErrorDocument 500`\. For example, if you name the attribute `[:apache][:custom][:error500]`, the corresponding line in `apache2.conf.erb` would then look something like the following:  

```
...
ErrorDocument 500 <%= node[:apache][:custom][:error500] %>
#ErrorDocument 404 /missing.html
...
```
You can then change the custom error message at any time by overriding `[:apache][:custom][:error500]`\. If you [use custom JSON to override the attribute](workingcookbook-json-override.md), you don't even need to touch the cookbook\.

To use the custom template, create a stack and install the cookbook\.

**To use the custom template**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/), and then choose **Add Stack**\.

1. Specify the following standard settings:
   + **Name** – ApacheTemplate
   + **Region** – US West \(Oregon\)
   + **Default SSH key** – An Amazon Elastic Compute Cloud \(Amazon EC2\) key pair

     If you need to create an Amazon EC2 key pair, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. Note that the key pair must belong to the same AWS region as the instance\.

   Choose **Advanced>>**, choose **Use custom Chef cookbooks**, to specify the following settings:
   + **Repository type** – **Http Archive**
   + **Repository URL** – The cookbook archive's URL that you recorded earlier

   Accept the default values for the other settings, and then choose **Add Stack** to create the stack\.

1. Choose **Add a layer**, and then [add a Java App Server layer](layers-java.md) to the stack with default settings\.

1. [Add a 24/7 instance](workinginstances-add.md) with default settings to the layer, and then start the instance\.

   A t2\.micro instance is sufficient for this example\.

1. After the instance is online, [connect to it with SSH](workinginstances-ssh.md)\. The `httpd.conf` file is in the `/etc/httpd/conf` directory\. The file should contain your custom `ErrorDocument` setting, which will look something like the following: 

   ```
   ...
   # Some examples:
   ErrorDocument 500 "A custom error message."
   #ErrorDocument 404 /missing.html
   #ErrorDocument 404 "/cgi-bin/missing_handler.pl"
   #ErrorDocument 402 http://www.example.com/subscription_info.html
   ...
   ```