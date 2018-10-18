# Apps<a name="workingapps"></a>

An AWS OpsWorks Stacks *app* represents code that you want to run on an application server\. The code itself resides in a repository such as an Amazon S3 archive; the app contains the information required to deploy the code to the appropriate application server instances\.

When you deploy an application, AWS OpsWorks Stacks triggers a Deploy event, which runs each layer's Deploy recipes\. AWS OpsWorks Stacks also installs [stack configuration and deployment attributes](workingcookbook-json.md) that contain all of the information needed to deploy the app, such as the app's repository and database connection data\.

You must implement custom recipes that retrieve the app's deployment data from the stack configuration and deployment attributes and handle the deployment tasks\. 

**Topics**
+ [Adding Apps](workingapps-creating.md)
+ [Deploying Apps](workingapps-deploying.md)
+ [Editing Apps](workingapps-editing.md)
+ [Connecting an Application to a Database Server](workingapps-connectdb.md)
+ [Using Environment Variables](apps-environment-vars.md)
+ [Passing Data to Applications](apps-data.md)
+ [Using Git Repository SSH Keys](workingapps-deploykeys.md)
+ [Using Custom Domains](workingapps-domains.md)
+ [Using SSL](workingsecurity-ssl.md)