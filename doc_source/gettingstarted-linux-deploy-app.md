# Step 6: Deploy the App to the Instance<a name="gettingstarted-linux-deploy-app"></a>

In this step, you will deploy the app from GitHub to the running instance\. \(For more information, see [Deploying Apps](workingapps-deploying.md)\.\) Before you deploy the app, you must specify the *recipe* to use to coordinate the deployment\. A recipe is a Chef concept\. Recipes are instructions, written with Ruby language syntax, that specify the resources to use and the order in which those resources are applied\. \(For more information, go to [About Recipes](https://docs.chef.io/recipes.html) on the [Learn Chef](https://learn.chef.io/) website\.\) 

**To specify the recipe to use to deploy the app to the instance**

1. In the service navigation pane, choose **Layers**\. The **Layers** page is displayed\.

1. For **MyLinuxDemoLayer**, choose **Recipes**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-layers-page-console.png)

   The **Layer MyLinuxDemoLayer** page is displayed with the **Recipes** tab open\.

1. For **Custom Chef Recipes**, for **Deploy**, type **nodejs\_demo::default**, and then press **Enter**\. `nodejs_demo` is the name of the cookbook and `default` is the name of the target recipe within the cookbook\. \(To explore the recipe's code, see [Learning More: Explore the Cookbook Used in This Walkthrough](gettingstarted-linux-explore-cookbook.md)\.\) Your results must match the following screenshot:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-recipes-page-console.png)

1. Choose **Save**\. AWS OpsWorks Stacks adds the recipe to the layer's Deploy lifecycle event\.

**To deploy the app to the instance**

1. In the service navigation pane, choose **Apps**\. The **Apps** page displays\. 

1. For **MyLinuxDemoApp**, for **Actions**, choose **deploy**, as displayed in the following screen shot:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-apps-page-console.png)

1. On the **Deploy App** page, leave the defaults for the following:
   + **Command** \(**Deploy**\)
   + **Comment** \(blank\)
   + **Settings**, **Advanced**, **Custom Chef JSON** \(blank\)
   + **Instances**, **Advanced** \(checked **Select all**, checked **MyLinuxDemoLayer**, checked **demo1**\)

1. Your results must match the following screenshot:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-deploy-app-console.png)

1. Choose **Deploy**\. The **Deployment MyLinuxDemoApp – deploy** page is displayed\. **Status** changes from **running** to **successful**\. A spinning circle displays next to **demo1**, which then changes to a green check mark\. Note that this process can take several minutes\. Do not proceed until you see both a **Status** of **successful** and the green check mark icon\.

1. Your results must match the following screenshot except of course for **Created at**, **Completed at**, **Duration**, and **User**\. If **status** is **failed**, then to troubleshoot, for **Log**, choose **show** to get details about the failure:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs-linux-app-deployed-console.png)

You have now successfully deployed the app to the instance\.

In the [next step](gettingstarted-linux-test-app.md), you will test the deployed app on the instance\.