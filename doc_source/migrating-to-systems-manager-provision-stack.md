# Step 5: Provision a CloudFormation stack<a name="migrating-to-systems-manager-provision-stack"></a>

**Note**  
You only need to complete this step if you set the `--provision-application` parameter for the script to `FALSE`\.

When you specify the `--provision-application` parameter with a value of `FALSE`, the script output provides the name and URL for the CloudFormation template\. This template represents a proposed replacement for your existing OpsWorks stack and layer\.

You can provision the template by using the Application Manager template library \(recommended\), or by using CloudFormation\. For more information about working with the template library, see [Working with the template library](https://docs.aws.amazon.com/systems-manager/latest/userguide/application-manager-working-templates-overview.html#application-manager-working-stacks-template-library-working) in the *AWS Systems Manager User Guide*\.