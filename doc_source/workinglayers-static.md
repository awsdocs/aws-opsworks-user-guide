# Static Web Server AWS OpsWorks Stacks Layer<a name="workinglayers-static"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The Static Web Server layer is an AWS OpsWorks Stacks layer that provides a template for instances to serve static HTML pages, which can include client\-side scripting\. This layer is based on [Nginx](http://nginx.org/en/)\.

**Installation**: Nginx is installed in `/usr/sbin/nginx`\.

The **Add Layer** page provides the following configuration options:

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Elastic Load Balancer**  
You can attach an Elastic Load Balancing load balancer to the layer's instances\.

You can modify some Nginx configuration settings by using custom JSON or a custom attributes file\. For more information, see [Overriding Attributes](workingcookbook-attributes.md)\. For a list of Apache attributes that can be overridden, see [nginx Attributes](attributes-recipes-nginx.md)\.

**Important**  
If your web application uses SSL, we recommend that you disable SSLv3 if possible to address the vulnerabilities described in [CVE\-2014\-3566](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566)\.   
To disable SSLv3, you must modify the Nginx server's `nginx.conf` file\. To do so, override the built\-in [nginx cookbook's](https://github.com/aws/opsworks-cookbooks/tree/release-chef-11.10/nginx) `nginx.conf.erb` template file, which the Rails App Server layer's Setup recipes use to create `nginx.conf`, and add the following directive:  

```
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
```
For more information on how to configure `nginx.conf`, see [Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html)\. For more information on how to override a built\-in template, see [Using Custom Templates](workingcookbook-template-override.md)\.