# Step 6: Verifying the app deployment in AWS OpsWorks Stacks<a name="other-services-cp-chef12-verify"></a>

To verify that CodePipeline deployed the Node\.js app to your stack, sign in to the instance you created in [Step 1: Create a stack, layer, and an instance in AWS OpsWorks Stacks](other-services-cp-chef12-stack.md)\. You should be able to see and use the Node\.js web app\.

**To verify the app deployment in your AWS OpsWorks Stacks instance**

1. Open the AWS OpsWorks console at [https://console\.aws\.amazon\.com/opsworks/](https://console.aws.amazon.com/opsworks/)\.

1. On the AWS OpsWorks Stacks dashboard, choose **CodePipelineDemo**, and then choose **Node\.js App Server**\.

1. In the navigation pane, choose **Instances**, and then choose the public IP address of the instance that you created to view the web app\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_instanceapp12.png)

   The app will be displayed in a new browser tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_successnode12.png)