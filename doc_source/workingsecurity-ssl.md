# Using SSL<a name="workingsecurity-ssl"></a>

To use SSL with your application, you must first obtain a digital server certificate from a Certificate Authority \(CA\)\. For simplicity, this walkthrough creates a certificate and then self\-signs it\. Self\-signed certificates are useful for learning and testing purposes, but you should always use a certificate signed by a CA for production stacks\. 

In this walkthrough, you'll do the following: 

1. Install and configure OpenSSL\.

1. Create a private key\.

1. Create a certificate signing request\.

1. Generate a self\-signed certificate\.

1. Edit the application with your certificate information\. 

**Important**  
If your application uses SSL, we recommend that you disable SSLv3, if possible, in your application server layers to address the vulnerabilities described in [CVE\-2014\-3566](https://cve.mitre.org/cgi-bin/cvename.cgi?name=cve-2014-3566)\. If your stack includes a Ganglia layer, you should disable SSL v3 for that layer too\. The details depend on the particular layer; for more information, see the following\.  
[Java App Server AWS OpsWorks Stacks Layer](layers-java.md)
[Node\.js App Server AWS OpsWorks Stacks Layer](workinglayers-node.md)
[PHP App Server AWS OpsWorks Stacks Layer](workinglayers-php.md)
[Rails App Server AWS OpsWorks Stacks Layer](workinglayers-rails.md)
[Static Web Server AWS OpsWorks Stacks Layer](workinglayers-static.md)
[Ganglia Layer](workinglayers-ganglia.md)

**Topics**
+ [Step 1: Install and Configure OpenSSL](#w4ab1c11c49c27c13)
+ [Step 2: Create a Private Key](#w4ab1c11c49c27c15)
+ [Step 3: Create a Certificate Signing Request](#w4ab1c11c49c27c17)
+ [Step 4: Submit the CSR to Certificate Authority](#w4ab1c11c49c27c19)
+ [Step 5: Edit the App](#w4ab1c11c49c27c21)

## Step 1: Install and Configure OpenSSL<a name="w4ab1c11c49c27c13"></a>

Creating and uploading server certificates requires a tool that supports the SSL and TLS protocols\. OpenSSL is an open\-source tool that provides the basic cryptographic functions necessary to create an RSA token and sign it with your private key\.

 The following procedure assumes that your computer does not already have OpenSSL installed\. 

**To install OpenSSL on Linux and Unix**

1. Go to [OpenSSL: Source, Tarballs](https://www.openssl.org/source/)\.

1. Download the latest source\.

1. Build the package\.

**To install OpenSSL on Windows**

1. If the Microsoft Visual C\+\+ 2008 Redistributable Package is not already installed on your system, download the [x64](http://www.microsoft.com/en-us/download/details.aspx?id=15336) or [x86](http://www.microsoft.com/en-us/download/details.aspx?id=29) version of the redistributable, whichever is appropriate for your environment\.

1. Run the installer and follow the instructions provided by the Microsoft Visual C\+\+ 2008 Redistributable Setup Wizard to install the redistributable\.

1. Go to [OpenSSL: Binary Distributions](https://www.openssl.org/community/binaries.html), click the appropriate version of the OpenSSL binaries for your environment, and save the installer locally\.

1. Run the installer and follow the instructions in the **OpenSSL Setup Wizard** to install the binaries\. 

Create an environment variable that points to the OpenSSL install point by opening a terminal or command window and using the following command lines\. 
+ On Linux and Unix

  ```
  export OpenSSL_HOME=path_to_your_OpenSSL_installation
  ```
+ On Windows

  ```
  set OpenSSL_HOME=path_to_your_OpenSSL_installation 
  ```

Add the OpenSSL binaries' path to your computer's path variable by opening a terminal or command window and using the following command lines\.
+ On Linux and Unix

  ```
  export PATH=$PATH:$OpenSSL_HOME/bin 
  ```
+ On Windows

  ```
  set Path=OpenSSL_HOME\bin;%Path% 
  ```

**Note**  
Any changes you make to the environment variables by using these command lines are valid only for the current command\-line session\.

## Step 2: Create a Private Key<a name="w4ab1c11c49c27c15"></a>

You need a unique private key to create your Certificate Signing Request \(CSR\)\. Create the key by using the following command line:

```
openssl genrsa 2048 > privatekey.pem
```

## Step 3: Create a Certificate Signing Request<a name="w4ab1c11c49c27c17"></a>

A Certificate Signing Request \(CSR\) is a file sent to a Certificate Authority \(CA\) to apply for a digital server certificate\. Create the CSR by using the following command line\.

```
openssl req -new -key privatekey.pem -out csr.pem
```

The command's output will look similar to the following:

```
You are about to be asked to enter information that will be incorporated 
	into your certificate request.
	What you are about to enter is what is called a Distinguished Name or a DN.
	There are quite a few fields but you can leave some blank
	For some fields there will be a default value,
	If you enter '.', the field will be left blank.
```

The following table can help you create your certificate request\.


**Certificate Request Data**  

| Name | Description | Example | 
| --- | --- | --- | 
| Country Name | The two\-letter ISO abbreviation for your country\. | US = United States | 
| State or Province | The name of the state or province where your organization is located\. This name cannot be abbreviated\. | Washington | 
| Locality Name | The name of the city where your organization is located\. | Seattle | 
| Organization Name | The full legal name of your organization\. Do not abbreviate your organization name\. | CorporationX | 
| Organizational Unit | \(Optional\) For additional organization information\. | Marketing | 
| Common Name | The fully qualified domain name for your CNAME\. You will receive a certificate name check warning if this is not an exact match\. | www\.example\.com | 
| Email address | The server administrator's email address | someone@example\.com | 

**Note**  
The Common Name field is often misunderstood and is completed incorrectly\. The common name is typically your host plus domain name\. It will look like "www\.example\.com" or "example\.com"\. You need to create a CSR using your correct common name\. 

## Step 4: Submit the CSR to Certificate Authority<a name="w4ab1c11c49c27c19"></a>

For production use, you would obtain a server certificate by submitting your CSR to a Certificate Authority \(CA\), which might require other credentials or proofs of identity\. If your application is successful, the CA returns digitally signed identity certificate and possibly a certificate chain file\. AWS does not recommend a specific CA\. For a partial listing of available CAs, see [Certificate Authority \- Providers](https://en.wikipedia.org/wiki/Certificate_authority#Providers) on Wikipedia\.

You can also generate a self\-signed certificate, which can be used for testing purposes only\. For this example, use the following command line to generate a self\-signed certificate\. 

```
openssl x509 -req -days 365 -in csr.pem -signkey privatekey.pem -out server.crt
```

The output will look similar to the following:

```
Loading 'screen' into random state - done
Signature ok
subject=/C=us/ST=washington/L=seattle/O=corporationx/OU=marketing/CN=example.com/emailAddress=someone@example.com
Getting Private key
```

## Step 5: Edit the App<a name="w4ab1c11c49c27c21"></a>

After you generate your certificate and sign it, update your app to enable SSL and provide your certificate information\. On the **Apps** page, choose an app to open the details page, and then click **Edit App**\. To enable SSL support, set **Enable SSL** to **Yes**, which displays the following configuration options\.

**SSL Certificate**  
Paste the contents of the public key certificate \(\.crt\) file into the box\. The certificate should look something like the following:  

```
-----BEGIN CERTIFICATE-----
MIICuTCCAiICCQCtqFKItVQJpzANBgkqhkiG9w0BAQUFADCBoDELMAkGA1UEBhMC
dXMxEzARBgNVBAgMCndhc2hpbmd0b24xEDAOBgNVBAcMB3NlYXR0bGUxDzANBgNV
BAoMBmFtYXpvbjEWMBQGA1UECwwNRGV2IGFuZCBUb29sczEdMBsGA1UEAwwUc3Rl
cGhhbmllYXBpZXJjZS5jb20xIjAgBgkqhkiG9w0BCQEWE3NhcGllcmNlQGFtYXpv
...
-----END CERTIFICATE-----
```
If you are using Nginx and you have a certificate chain file, you should append the contents to the public key certificate file\.
If you are updating an existing certificate, do the following:  
+ Choose **Update SSL certificate** to update the certificate\.
+ If the new certificate does not match the existing private key, choose **Update SSL certificate key**\.
+ If the new certificate does not match the existing certificate chain, choose **Update SSL certificates**\.

**SSL Certificate Key**  
Paste the contents of the private key file \(\.pem file\) into the box\. It should look something like the following:  

```
----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQC0CYklJY5r4vV2NHQYEpwtsLuMMBhylMrgBShKq+HHVLYQQCL6
+wGIiRq5qXqZlRXje3GM5Jvcm6q0R71MfRIl1FuzKyqDtneZaAIEYniZibHiUnmO
/UNqpFDosw/6hY3ONk0fSBlU4ivD0Gjpf6J80jL3DJ4R23Ed0sdL4pRT3QIDAQAB
AoGBAKmMfWrNRqYVtGKgnWB6Tji9QrKQLMXjmHeGg95mppdJELiXHhpMvrHtpIyK
...
-----END RSA PRIVATE KEY-----
```

**SSL certificates of Certification Authorities**  
If you have a certificate chain file, paste the contents into the box\.  
If you are using Nginx, you should leave this box empty\. If you have a certificate chain file, append it to the public key certificate file in **SSL Certificate**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/app_ssl_settings.png)

After you click **Save**, [redeploy the application](workingapps-deploying.md) to update your online instances\.

For the [built\-in application server layers](workingcookbook-json.md#workingcookbook-json-deploy), AWS OpsWorks Stacks automatically updates the server configuration\. After deployment is finished, you can verify that your OpenSSL installation worked, as follows\.

**To verify an OpenSSL installation**

1. Go to the **Instances** page\.

1. Run the app by clicking the application server instance's IP address or, if you are using a load balancer, the load balancer's IP address\.

1. Change the IP address prefix from **http://** to **https://** and refresh the browser to verify the page loads correctly with SSL\.

If your app does not run as expected, or the webpage does not work as expected, see the "Using the OpenSSL application" section of the [OpenSSL FAQ](https://www.openssl.org/docs/faq.html#USER) for troubleshooting information\. Users who have configured apps to run in Mozilla Firefox sometimes get the following certificate error: `SEC_ERROR_UNKNOWN_ISSUER`\. This error can be caused by certificate\-replacement functionality in your organization's antivirus and antimalware programs, by some types of network traffic monitoring and filtering software, or by malware\. For more information about how to troubleshoot this error, see [How to troubleshoot security error codes on secure websites](https://support.mozilla.org/en-US/kb/error-codes-secure-websites?redirectlocale=en-US&redirectslug=troubleshoot-SEC_ERROR_UNKNOWN_ISSUER#w_monitoringfiltering-in-corporate-networks) on the Mozilla Firefox Support website\.

For all other layers, including custom layers, AWS OpsWorks Stacks simply adds the SSL settings to the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy)\. You must implement a custom recipe to retrieve the information from the node object and configure the server appropriately\.