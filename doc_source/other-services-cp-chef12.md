# AWS CodePipeline with AWS OpsWorks Stacks \- Chef 12 Stacks<a name="other-services-cp-chef12"></a>

[AWS CodePipeline](https://aws.amazon.com/codepipeline/) lets you create continuous delivery pipelines that track code changes from sources such as CodeCommit, Amazon Simple Storage Service \(Amazon S3\), or [GitHub](https://github.com/)\. The example in this topic describes how to create and use a simple pipeline from CodePipeline as a deployment tool for code that you run on AWS OpsWorks Stacks layers\. In this example, you create a pipeline for a simple [Node\.js app](samples/opsworks-nodejs-demo-app.zip), and then instruct AWS OpsWorks Stacks to run the app on all of the instances in a layer in a Chef 12 stack \(in this case, a single instance\)\.

**Note**  
This topic describes how to use a pipeline to run and update an app on a Chef 12 stack\. For information about how to use a pipeline to run and update an app on a Chef 11\.10 stack, see [AWS CodePipeline with AWS OpsWorks Stacks \- Chef 11 Stacks](other-services-cp-chef11.md)\. Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**Topics**
+ [Prerequisites](#w4ab1c11c65c17b9b9)
+ [Other Supported Scenarios](#w4ab1c11c65c17b9c11)
+ [Step 1: Create a stack, layer, and an instance in AWS OpsWorks Stacks](other-services-cp-chef12-stack.md)
+ [Step 2: Configure your stack and layer to use custom cookbooks](other-services-cp-stackconfig.md)
+ [Step 3: Upload app code to an Amazon S3 bucket](other-services-cp-chef12-s3.md)
+ [Step 4: Add your app to AWS OpsWorks Stacks](other-services-cp-chef12-addapp.md)
+ [Step 5: Create a pipeline in CodePipeline](other-services-cp-chef12-pipeline.md)
+ [Step 6: Verifying the app deployment in AWS OpsWorks Stacks](other-services-cp-chef12-verify.md)
+ [Step 7 \(Optional\): Update the app code to see CodePipeline redeploy your app automatically](other-services-cp-chef12-update.md)
+ [Step 8 \(Optional\): Clean up resources](other-services-cp-chef12-cleanup.md)

## Prerequisites<a name="w4ab1c11c65c17b9b9"></a>

Before you start this walkthrough, be sure that you have administrator permissions to do all of the following tasks\. You can be a member of a group that has the **AdministratorAccess** policy applied, or you can be a member of a group that has the permissions and policies shown in the following table\. As a security best practice, you should belong to a group that has permissions to do the following tasks, instead of assigning required permissions to individual user accounts\.

For more information about creating a security group in IAM and assigning permissions to the group, see [Creating Your First IAM User and Administrators Group](http://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\. For more information about managing AWS OpsWorks Stacks permissions, see [Best Practices: Managing Permissions](http://docs.aws.amazon.com/opsworks/latest/userguide/best-practices-permissions.html)\. For more information about managing CodePipeline permissions, see [AWS CodePipeline Access Permissions Reference](http://docs.aws.amazon.com/codepipeline/latest/userguide/access-permissions.html) in the CodePipeline documentation\.


| Permissions | Recommended Policy to Attach to Group | 
| --- | --- | 
|  Create and edit stacks, layers, and instances in AWS OpsWorks Stacks\.  | AWSOpsWorksFullAccess | 
|  Create, edit, and run templates in AWS CloudFormation\.  | AmazonCloudFormationFullAccess | 
|  Create, edit, and access Amazon S3 buckets\.  | AmazonS3FullAccess | 
|  Create, edit, and run pipelines in CodePipeline, especially pipelines that use AWS OpsWorks Stacks as the provider\.  | AWSCodePipelineFullAccess | 

You must also have an Amazon EC2 key pair\. You will be prompted to provide the name of this key pair when you run the AWS CloudFormation template that creates the sample stack, layer, and instance in this walkthrough\. For more information about obtaining a key pair in the Amazon EC2 console, see [Create a Key Pair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html#create-a-key-pair) in the Amazon EC2 documentation\. The key pair must be in the US East \(N\. Virginia\) Region\. You can use an existing key pair if you already have one in that region\.

## Other Supported Scenarios<a name="w4ab1c11c65c17b9c11"></a>

This walkthrough creates a simple pipeline that includes one **Source** and one **Deploy** stage\. However, you can create more complex pipelines that use AWS OpsWorks Stacks as a provider\. The following are examples of supported pipelines and scenarios:
+ You can edit a pipeline to add a Chef cookbook to the **Source** stage and an associated target for updated cookbooks to the **Deploy** stage\. In this case, you add a **Deploy** action that triggers the updating of your cookbooks when you make changes to the source\. The updated cookbook is deployed before your app\.
+ You can create a complex pipeline, with custom cookbooks and multiple apps, and deploy to an AWS OpsWorks Stacks stack\. The pipeline tracks changes to both the application and cookbook sources, and redeploys when you have made changes\. The following shows an example of a similar, complex pipeline:  
![\[\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cp_integ_complexpipeline.png)

For more information about working with CodePipeline, see the [CodePipeline User Guide](http://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)\.