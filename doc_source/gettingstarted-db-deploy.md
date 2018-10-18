# Step 3\.5: Deploy SimplePHPApp, Version 2<a name="gettingstarted-db-deploy"></a>

The final step is to deploy the new version of SimplePHPApp\.

**To deploy SimplePHPApp**

1. On the **Apps** page, click **deploy** in the **SimplePHPApp** app's **Actions**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gsb6aa.png)

1. Accept the defaults and click **Deploy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs17a.png)

   When you click **Deploy** on the **Deploy App** page, you trigger a Deploy lifecycle event, which notifies the agents to run their Deploy recipes\. By default, you trigger the event on all of the stack's instances\. The built\-in Deploy recipes deploy the app only to the appropriate instances for the app type, PHP App Server instances in this case\. However, it is often useful to trigger the Deploy event on other instances, to allow them to respond to the app deployment\. In this case, you also want to trigger Deploy on the MySQL instance to set up the database\. 

   Note the following:
   + The agent on the PHP App Server instance runs the layer's built\-in recipe, followed by `appsetup.rb`, which configures the app's database connection\.
   + The agent on the MySQL instance doesn't install anything, but it runs `dbsetup.rb` to create the urler table\.

   When the deployment is complete, the **Status** will change to **successful** on the **Deployment** page\.