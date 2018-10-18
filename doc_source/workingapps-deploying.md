# Deploying Apps<a name="workingapps-deploying"></a>

The primary purpose of deployment is to deploy application code and related files to application server instances\. The deployment operation is handled by each instance's Deploy recipes, which are determined by the instance's layer\.

When you start an instance, after the Setup recipes complete, AWS OpsWorks Stacks automatically runs the instance's Deploy recipes\. However, when you add or modify an app, you must deploy it manually to any online instances\. You must have Manage or Deploy permissions to deploy an app\. For more information, see [Managing User Permissions](opsworks-security-users.md)\.

**To deploy an app**

1. On the **Apps** page, click the app's **deploy** action\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/apps_with_content.png)
**Note**  
You can also deploy an app by clicking **Deployments **in the navigation pane\. On the **Deployments & Commands** page, click **Deploy an app** When you do this, you can also choose which app to deploy\.

1. Specify the following:
   + \(Required\) Set **Command:** to **deploy**, if it is not already selected\.
   + \(Optional\) Include a comment\. 

1. Click **Advanced >>** to specify custom JSON\. AWS OpsWorks Stacks adds a set of [stack configuration and deployment attributes](workingcookbook-json.md) to the node object\. The `deploy` attributes contain the deployment details and can be used by Deploy recipes to handle installation and configuration\. On Linux stacks, you can use the custom JSON field to [override default AWS OpsWorks Stacks settings](workingcookbook-json-override.md) or pass custom settings to your custom recipes\. For more information about how to use custom JSON, see [Using Custom JSON](workingstacks-json.md)\.
**Note**  
If you specify custom JSON here, it is added to the stack configuration and deployment attributes for this deployment only\. If you want to add custom JSON permanently, you must [add it to the stack](workingstacks-json.md)\. Custom JSON is limited to 120 KB\. If you need more capacity, we recommend storing some of the data on Amazon S3\. Your custom recipes can then use the AWS CLI or [AWS SDK for Ruby](http://aws.amazon.com/documentation/sdk-for-ruby/) to download the data from the bucket to your instance\. For an example, see [Using the SDK for Ruby](cookbooks-101-opsworks-s3.md)\.

1. Under **Instances**, click **Advanced >>** and specify which instances to run the deploy command on\.

   The deploy command triggers a Deploy event, which runs the deploy recipes on the selected instances\. The deploy recipes for the associated application server download the code and related files from the repository and install them on the instance, so you typically select all of the associated application server instances\. However, other instance types might require some configuration changes to accommodate the new app, so it is often useful to run deploy recipes on those instances as well\. Those recipes update the configuration as needed but do not install the app's files\. For more information about recipes, see [Cookbooks and Recipes](workingcookbook.md)\.

1. Click **Deploy** to run the deploy recipes on the specified instances, which displays the Deployment page\. When the process is complete, AWS OpsWorks Stacks marks the app with a green check to indicate successful deployment\. If deployment fails, AWS OpsWorks Stacks marks the app with a red X\. In that case, you can go to the **Deployments** page and examine the deployment log for more information\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/deployed_app.png)

**Note**  
When you deploy an update to a JSP app, Tomcat might not recognize the update and instead continue to run the existing app version\. This can happen, for example, if you deploy your app as a \.zip file that contains only a JSP page\. To ensure that Tomcat runs the most recently deployed version, the project's root directory should include a WEB\-INF directory that contains a `web.xml` file\. A `web.xml` file can contain a variety of content, but the following is sufficient to ensure that Tomcat recognizes updates and runs the currently deployed app version\. You don't have to change the version for each update\. Tomcat will recognize the update even if the version hasn't changed\.  

```
<context-param>
  <param-name>appVersion</param-name>
  <param-value>0.1</param-value>
</context-param>
```

## Other Deployment Commands<a name="workingapps-deploying-other"></a>

The **Deploy app** page includes several other commands for managing your apps and the associated servers\. Of the following commands, only **Undeploy** is available for apps on Chef 12 stacks\.

**Undeploy**  
Triggers an Undeploy [lifecycle event](workingcookbook-events.md), which runs the undeploy recipes to remove all versions of the app from the specified instances\.

**Rollback**  
Restores the previously deployed app version\. For example, if you have deployed the app three times and then run **Rollback**, the server will serve the app from the second deployment\. If you run **Rollback** again, the server will serve the app from the first deployment\. By default, AWS OpsWorks Stacks stores the five most recent deployments, which allows you to roll back up to four versions\. If you exceed the number of stored versions, the command fails and leaves the oldest version in place\. This command is not available in Chef 12 stacks\.

**Start Web Server**  
Runs recipes that start the application server on the specified instances\. This command is not available in Chef 12 stacks\.

**Stop Web Server**  
Runs recipes that stop the application server on the specified instances\. This command is not available in Chef 12 stacks\.

**Restart Web Server**  
Runs recipes that restart the application server on the specified instances\. This command is not available in Chef 12 stacks\.

**Important**  
**Start Web Server**, **Stop Web Server**, **Restart Web Server**, and **Rollback** are essentially customized versions of the [Execute Recipes stack command](workingstacks-commands.md)\. They run a set of recipes that perform the task on the specified instances\.  
These commands do not trigger a lifecycle event, so you cannot hook them to run custom code\.
These commands work only for the built\-in [application server layers](workinglayers-servers.md)\.  
In particular, these commands have no effect on custom layers, even if they support an application server\. To start, stop, or restart servers on a custom layer, you must implement custom recipes to perform these tasks and use the [Execute Recipes stack command](workingstacks-commands.md) to run them\. For more information on how to implement and install custom recipes, see [Cookbooks and Recipes](workingcookbook.md)\.