# Using Custom Domains<a name="workingapps-domains"></a>

If you host a domain name with a third party, you can map that domain name to an app\. The basic procedure is as follows:

1. Create a subdomain with your DNS registrar and map it to your load balancer's Elastic IP address or your app server's public IP address\.

1. Update your app's configuration to point to the subdomain and redeploy the app\. 

**Note**  
Make sure you forward your unqualified domain name \(such as myapp1\.example\.com\) to your qualified domain name \(such as www\.myapp1\.example\.com\) so that both map to your app\. 

When you configure a domain for an app, it is listed as a server alias in the server's configuration file\. If you are using a load balancer, the load balancer checks the domain name in the URL as requests come in and redirects the traffic based on the domain\.

**To map a subdomain to an IP address**

1. If you are using a load balancer, on the **Instances** page, click the load balancer instance to open its details page and get the instance's **Elastic IP** address\. Otherwise, get the public IP address from the application server instance's details page\. 

1. Follow the directions provided by your DNS registrar to create and map your subdomain to the IP address from Step 1\.

**Note**  
If the load balancer instance terminates at some point, you are assigned a new Elastic IP address\. You need to update your DNS registrar settings to map to the new Elastic IP address\.

AWS OpsWorks Stacks simply adds the domain settings to the app's [`deploy` attributes](workingcookbook-json.md#workingcookbook-json-deploy)\. You must implement a custom recipe to retrieve the information from the node object and configure the server appropriately\. For more information, see [Cookbooks and Recipes](workingcookbook.md)\.