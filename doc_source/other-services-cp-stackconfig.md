# Step 2: Configure your stack and layer to use custom cookbooks<a name="other-services-cp-stackconfig"></a>

Chef 12 stacks in AWS OpsWorks Stacks require your own or community\-created cookbooks to build custom application layers\. For this walkthrough, you can point to a repository that contains a set of [Chef cookbooks](https://docs.chef.io/cookbooks.html) and Chef recipes\. These recipes install the Node\.js package and its dependencies on your instance\. You will use other Chef recipes to deploy the Node\.js app that you will prepare in [Step 4: Add your app to AWS OpsWorks Stacks](other-services-cp-chef12-addapp.md)\. The Chef recipe that you specify in this step runs every time a new version of your application is deployed by CodePipeline\.

1. In the AWS OpsWorks Stacks console, open the stack that you created in [Step 1: Create a stack, layer, and an instance in AWS OpsWorks Stacks](other-services-cp-chef12-stack.md)\. Choose **Stack Settings**, and then choose **Edit**\.

1. Set **Use custom Chef cookbooks** to **Yes**\. This shows related custom cookbook settings\.

1. From the **Repository type** drop\-down list, choose **S3 Archive**\. To work with both CodePipeline and AWS OpsWorks, your cookbook source must be S3\.

1. For **Repository URL**, specify `https://s3.amazonaws.com/opsworks-codepipeline-demo/opsworks-nodejs-demo-cookbook.zip`\. Your settings should resemble the following\.  
![\[Use custom Chef cookbooks setttings.\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_usecustomcook.png)

1. Choose **Save**\.

1. In the navigation pane, choose **Layers**\.

1. Choose **Settings** for the layer you created in [Step 1: Create a stack, layer, and an instance in AWS OpsWorks Stacks](other-services-cp-chef12-stack.md)\.

1. On the **General Settings** tab, be sure that the layer name is **Node\.js App Server**, and the layer short name is **app1**\. Choose **Recipes**\.

1. On the **Recipes** tab, specify **nodejs\_demo** as the recipe you want to run during the **Deploy** lifecycle event\. Choose **Save**\.

   Your recipe settings should resemble the following:  
![\[Layer recipe settings\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_layerconfig.png)

1. On the **Security** tab, from the **Security groups** drop\-down list, choose the **AWS\-OpsWorks\-Webapp** security group\.

1. Choose **Save**\.