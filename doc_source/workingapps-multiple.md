# Running Multiple Applications on the Same Application Server<a name="workingapps-multiple"></a>

**Note**  
The information in this topic does not apply to Node\.js apps\.

If you have multiple applications of the same type, it is sometimes more cost\-effective to run them on the same application server instances\.

**To run multiple applications on the same server**

1. Add an app to the stack for each application\.

1. Obtain a separate subdomain for each app and map the subdomains to the application server's or load balancer's IP address\.

1. Edit each app's configuration to specify the appropriate subdomain\.

For more information on how to perform these tasks, see [Using Custom Domains](workingapps-domains.md)\.

**Note**  
If your application servers are running multiple HTTP applications, you can use Elastic Load Balancing for load\-balancing\. For multiple HTTPS applications, you must either terminate the SSL connection at the load balancer or create a separate stack for each application\. HTTPS requests are encrypted, which means that if you terminate the SSL connection at the servers, the load balancer cannot check the domain name to determine which application should handle the request\.