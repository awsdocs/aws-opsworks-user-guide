# Built\-in Cookbook Attributes<a name="attributes-recipes"></a>

**Note**  
Most of these attributes are available only on Linux stacks\.

Most of the built\-in recipes have one or more [attributes files](workingcookbook-installingcustom-components-attributes.md) that define various settings\. You can access these settings in your custom recipes and use custom JSON to override them\. You typically need to access or override attributes that control the configuration of the various server technologies that are supported by AWS OpsWorks Stacks\. This section summarizes those attributes\. The complete attributes files, and the associated recipes and templates, are available at [https://github\.com/aws/opsworks\-cookbooks\.git](https://github.com/aws/opsworks-cookbooks.git)\.

**Note**  
All built\-in recipe attributes are `default` type\.

**Topics**
+ [apache2 Attributes](attributes-recipes-apache.md)
+ [deploy Attributes](attributes-recipes-deploy.md)
+ [haproxy Attributes](attributes-recipes-haproxy.md)
+ [memached Attributes](attributes-recipes-mem.md)
+ [mysql Attributes](attributes-recipes-mysql.md)
+ [nginx Attributes](attributes-recipes-nginx.md)
+ [opsworks\_berkshelf Attributes](attributes-recipes-berkshelf.md)
+ [opsworks\_java Attributes](attributes-recipes-java.md)
+ [passenger\_apache2 Attributes](attributes-recipes-passenger.md)
+ [ruby Attributes](attributes-recipes-ruby.md)
+ [unicorn Attributes](attributes-recipes-unicorn.md)