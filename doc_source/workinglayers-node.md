# Node\.js App Server AWS OpsWorks Stacks Layer<a name="workinglayers-node"></a>

**Note**  
This layer is available only for Linux\-based stacks\.

The Node\.js App Server layer is an AWS OpsWorks Stacks layer that provides a blueprint for instances that function as [Node\.js](http://nodejs.org/) application servers\. AWS OpsWorks Stacks also installs [Express](http://expressjs.com/), so the layer's instances support both standard and Express applications\.

**Installation**: Node\.js is installed in `/usr/local/bin/node`\.

The **Add Layer** page provides the following configuration options:

**Node\.js version**  
For a list of currently supported versions, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.

**Custom security groups**  
This setting appears if you chose to not automatically associate a built\-in AWS OpsWorks Stacks security group with your layers\. You must specify which security group to associate with the layer\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Elastic Load Balancer**  
You can attach an Elastic Load Balancing load balancer to the layer's instances\.

**Important**  
If your Node\.js application uses SSL, we recommend that you disable SSLv3 if possible to address the vulnerabilities described in [CVE\-2015\-8027](http://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-8027)\. To do so, you must set **Node\.js version** to `0.12.9`\.

## Deploying Node\.js Apps<a name="w4ab1c11c63b7c19c19c17c15"></a>

For a detailed walkthrough of how to implement a simple Node\.js application for AWS OpsWorks Stacks and deploy it to a stack, see [Creating Your First Node\.js Stack](gettingstarted-node.md)\. In general, Node\.js applications for AWS OpsWorks Stacks should meet the following conditions:
+ The main file must be named `server.js` and reside in the deployed application's root directory\.
+ [Express](http://expressjs.com/) apps must include a `package.json` file in the application's root directory\.
+ By default, the application must listen on port 80 \(HTTP\) or port 443 \(HTTPS\)\.

  It is possible to listen on other ports, but the Node\.js App Server layer's built\-in security group, **AWS\-OpsWorks\-nodejs\-App\-Server**, allows inbound user traffic only to ports 80, 443, and 22 \(SSH\)\. To allow inbound user traffic to other ports, [create a security group](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) with appropriate inbound rules and [assign it to the Node\.js App Server layer](workinglayers-basics-edit.md#workinglayers-basics-edit-security)\. Do not modify inbound rules by editing the built\-in security group\. Each time you create a stack, AWS OpsWorks Stacks overwrites the built\-in security groups with the standard settings, so any changes that you make will be lost\.

**Note**  
AWS OpsWorks Stacks sets the PORT environment variable to 80 \(default\) or 443 \(if you enable SSL\), so you can use the following code to listen for requests\.  

```
app.listen(process.env.PORT);
```

If you [configure a Node\.js app to support SSL](workingapps-creating.md#workingapps-creating-domain-ssl), you must specify the key and certificates\. AWS OpsWorks Stacks puts the data for each application server instance as separate files in the `/srv/www/app_shortname/shared/config` directory, as follows\.
+ `ssl.crt` – the SSL certificate\.
+ `ssl.key` – the SSL key\.
+ `ssl.ca` – the chain certificate, if you have specified one\.

Your application can obtain the SSL key and certificates from those files\.