# Java App Server AWS OpsWorks Stacks Layer<a name="layers-java"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The Java App Server layer is an AWS OpsWorks Stacks layer that provides a blueprint for instances that function as Java application servers\. This layer is based on [Apache Tomcat 7\.0](http://tomcat.apache.org/) and [Open JDK 7](http://openjdk.java.net/)\. AWS OpsWorks Stacks also installs the Java connector library, which allows Java apps to use a JDBC `DataSource` object to connect to a back end data store\.

**Installation**: Tomcat is installed in `/usr/share/tomcat7`\.

The **Add Layer** page provides the following configuration options:

**Java VM Options**  
You can use this setting to specify custom Java VM options; there are no default options\. For example, a common set of options is `-Djava.awt.headless=true -Xmx128m -XX:+UseConcMarkSweepGC`\. If you use **Java VM Options**, make sure that you pass a valid set of options; AWS OpsWorks Stacks does not validate the string\. If you attempt to pass an invalid option, the Tomcat server typically fails to start, which causes setup to fail\. If that happens, you can examine the instance's setup Chef log for details\. For more information on how to view and interpret Chef logs, see [Chef Logs](troubleshoot-debug-log.md)\.

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Elastic Load Balancer**  
You can attach an Elastic Load Balancing load balancer to the layer's instances\. For more information, see [Elastic Load Balancing Layer](layers-elb.md)\.

You can specify other configuration settings by using custom JSON or a custom attributes file\. For more information, see [Custom Configuration](layers-java-config.md)\.

**Important**  
If your Java application uses SSL, we recommend that you disable SSLv3 if possible to address the vulnerabilities described in [CVE\-2014\-3566](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566)\. For more information, see [Disabling SSLv3 for Apache Servers](#layers-java-sslv3)\.

**Topics**
+ [Disabling SSLv3 for Apache Servers](#layers-java-sslv3)
+ [Custom Configuration](layers-java-config.md)
+ [Deploying Java Apps](layers-java-deploy.md)

## Disabling SSLv3 for Apache Servers<a name="layers-java-sslv3"></a>

To disable SSLv3, you must modify the Apache server's `ssl.conf` file's `SSLProtocol` setting\. To do so, you must override the built\-in [apache2 cookbook's](https://github.com/aws/opsworks-cookbooks/tree/release-chef-11.10/apache2) `ssl.conf.erb` template file, which the Java App Server layer's Setup recipes use to create `ssl.conf`\. The details depend on which operating system you specify for the layer's instances\. The following summarizes the required modifications for Amazon Linux and Ubuntu systems\. SSLv3 is automatically disabled for Red Hat Enterprise Linux \(RHEL\) systems\. For more information on how to override a built\-in template, see [Using Custom Templates](workingcookbook-template-override.md)\.

**Amazon Linux and Ubuntu 12\.04 LTS**  
The `ssl.conf.erb` file for these operating systems is in the `apache2` cookbook's `apache2/templates/default/mods` directory\. The following shows the relevant part of the built\-in file\.  

```
...
#SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP:+eNULL

# enable only secure protocols: SSLv3 and TLSv1, but not SSLv2
SSLProtocol all -SSLv2
</IfModule>
```
Override `ssl.conf.erb` and modify the `SSLProtocol` setting as follows\.  

```
...
#SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP:+eNULL

# enable only secure protocols: SSLv3 and TLSv1, but not SSLv2
SSLProtocol all -SSLv3 -SSLv2
</IfModule>
```

**Ubuntu 14\.04 LTS**  
The `ssl.conf.erb` file for this operating system is in the `apache2` cookbook's `apache2/templates/ubuntu-14.04/mods` directory\. The following shows the relevant part of the built\-in file\.  

```
...
# The protocols to enable.
# Available values: all, SSLv3, TLSv1, TLSv1.1, TLSv1.2
# SSL v2 is no longer supported
SSLProtocol all
...
```
Change this setting to the following\.  

```
...
# The protocols to enable.
# Available values: all, SSLv3, TLSv1, TLSv1.1, TLSv1.2
# SSL v2 is no longer supported
SSLProtocol all -SSLv3 -SSLv2
...
```