# Step 6: Review the provisioned resources<a name="migrating-to-systems-manager-provision-resources"></a>

You are now ready to review the provisioned resources\.

1. Review resources for the provisioned stack using the AWS CloudFormation console\.

   1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/) and choose **Stacks**\.

   1. On the **Stacks** page, choose the stack, and then choose the **Resources** tab\.

   1. On the **Resources** tab, review the listed resources for your stack\. The list of resources includes an EC2 Auto Scaling group, which you can review in the Auto Scaling console, or AWS CLI\.

1. Review the resources for the application using Systems Manager Application Manager\.

   1.  Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. 

   1. In the navigation pane, choose **Application Manager**\.

   1.  In the **Applications** section, choose **Custom applications**\. 

   1.  Choose the application in the list\. Application Manager opens the **Overview** tab\. 

   1.  Choose the **Resources** tab\. The **Resources** tab shows all resources that were migrated for your OpsWorks stack and layer\. The application name includes the name of the OpsWorks stack, and is formatted as *app*\-*stack\-name*\-*suffix* where *suffix* represents the first six characters of the stack ID\. For more information about viewing resources in Application Manager, see [Viewing application resources](https://docs.aws.amazon.com/systems-manager/latest/userguide/application-manager-working-viewing-resources.html) in the *AWS Systems Manager User Guide*\. 