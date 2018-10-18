# Ganglia Layer<a name="workinglayers-ganglia"></a>

**Note**  
This layer is available only for for Chef 11 and earlier Linux\-based stacks\.

AWS OpsWorks Stacks sends all of your instance and volume metrics to [Amazon CloudWatch](http://aws.amazon.com/documentation/cloudwatch/), making it easy to view graphs and set alarms to help you troubleshoot and take automated action based on the state of your resources\. You can also use the Ganglia AWS OpsWorks Stacks layer for additional application monitoring options such as storing your chosen metrics\.

The Ganglia layer is a blueprint for an instance that monitors your stack by using [Ganglia](http://ganglia.sourceforge.net/) distributed monitoring\. A stack usually has only one Ganglia instance\. The Ganglia layer includes the following optional configuration settings:

**Ganglia URL**  
The statistic's URL path\. The complete URL is http://*DNSName**URLPath*, where *DNSName* is the associated instance's DNS name\. The default *URLPath* value is "/ganglia" which corresponds to something like: http://ec2\-54\-245\-151\-7\.us\-west\-2\.compute\.amazonaws\.com/ganglia\.

**Ganglia user name**  
A user name for the statistics web page\. You must provide the user name when you view the page\. The default value is "opsworks"\. 

**Ganglia password**  
A password that controls access to the statistics web page\. You must provide the password when you view the page\. The default value is a randomly generated string\.   
Record the password for later use; AWS OpsWorks Stacks does not allow you to view the password after you create the layer\. However, you can update the password by going to the layer's Edit page and clicking **Update password**\.

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Elastic Load Balancer**  
You can attach an Elastic Load Balancing load balancer to the layer's instances\.

**Important**  
If your stack includes a Ganglia layer, we recommend that you disable SSLv3 if possible for that layer to address the vulnerabilities described in [CVE\-2014\-3566](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566)\. To do so, you must override the Apache server's `ssl.conf.erb` template to modify the `SSLProtocol` setting\. For details, see [Disabling SSLv3 for Apache Servers](layers-java.md#layers-java-sslv3)\.

## View the Ganglia Statistics<a name="w4ab1c11c63b7c19c27b9c13"></a>

AWS OpsWorks Stacks recipes install a low\-overhead Ganglia client on every instance\. If your stack includes a Ganglia layer, the Ganglia client automatically starts reporting to the Ganglia as soon as the instance comes on line\. The Ganglia uses the client data to compute a variety of statistics and displays the results graphically on its statistics web page\.

**To view Ganglia statistics**

1. On the **Layers** page, click Ganglia to open the layer's details page\.

1. In the navigation pane, click **Instances**\. Under **Ganglia**, click the instance name\.

1.  Copy the instance's **Public DNS** name\.

1. Use the DNS name to construct the statistics URL, which will look something like: http://ec2\-54\-245\-151\-7\.us\-west\-2\.compute\.amazonaws\.com/ganglia\.

1. Paste the complete URL into your browser, navigate to the page, and enter the Ganglia user name and password to display the page\. An example follows\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/ganglia_stats.png)