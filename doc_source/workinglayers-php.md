# PHP App Server AWS OpsWorks Stacks Layer<a name="workinglayers-php"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The PHP App Server layer is an AWS OpsWorks Stacks layer that provides a blueprint for instances that function as PHP application servers\. The PHP App Server layer is based on [Apache2](http://httpd.apache.org/) with `mod_php` and has no standard configuration options\. The PHP and Apache version depends on which [operating system](workinginstances-os.md) you specify for the layer's instances\. 


| Operating System | PHP Version | Apache Version | 
| --- | --- | --- | 
| Amazon Linux 2018\.03 | 5\.3 | 2\.2 | 
| Amazon Linux 2017\.09 | 5\.3 | 2\.2 | 
| Amazon Linux 2017\.03 | 5\.3 | 2\.2 | 
| Amazon Linux 2016\.09 | 5\.3 | 2\.2 | 
| Amazon Linux 2016\.03 | 5\.3 | 2\.2 | 
| Amazon Linux 2015\.09 | 5\.3 | 2\.2 | 
| Amazon Linux 2015\.03 | 5\.3 | 2\.2 | 
| Amazon Linux 2014\.09 | 5\.3 | 2\.2 | 
| Ubuntu 14\.04 LTS | 5\.5 | 2\.4 | 
| Ubuntu 12\.04 LTS | 5\.3 | 2\.2 | 

**Installation**: AWS OpsWorks Stacks uses the instance's package installer to install Apache2 and `mod_php` in their default locations\. For more information about installation, see [Apache](http://httpd.apache.org/)\.

The **Add Layer** page provides the following configuration options:

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Elastic Load Balancer**  
You can attach an Elastic Load Balancing load balancer to the layer's instances\.

You can modify some Apache configuration settings by using custom JSON or a custom attributes file\. For more information, see [Overriding Attributes](workingcookbook-attributes.md)\. For a list of Apache attributes that can be overridden, see [apache2 Attributes](attributes-recipes-apache.md)\.

For an example of how to deploy a PHP App, including how to connect the app to a backend database, see [Getting Started with Chef 11 Linux Stacks](gettingstarted.md)\.

**Important**  
If your PHP application uses SSL, we recommend that you disable SSLv3 if possible to address the vulnerabilities described in [CVE\-2014\-3566](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566)\. To do so, you must modify the `SSLProtocol` setting in the Apache server's `ssl.conf` file\. For more information on how to modify this setting, see [Disabling SSLv3 for Apache Servers](layers-java.md#layers-java-sslv3)\.