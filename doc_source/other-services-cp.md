# Using AWS CodePipeline with AWS OpsWorks Stacks<a name="other-services-cp"></a>

[AWS CodePipeline](https://aws.amazon.com/codepipeline/) lets you create continuous delivery pipelines that track code changes from sources such as CodeCommit, Amazon Simple Storage Service \(Amazon S3\), or [GitHub](https://github.com/)\. You can use CodePipeline to automate the release of your Chef cookbooks and application code to AWS OpsWorks Stacks, on Chef 11\.10, Chef 12, and Chef 12\.2 stacks\. Examples in this section describe how to create and use a simple pipeline from CodePipeline as a deployment tool for code that you run on AWS OpsWorks Stacks layers\.

**Note**  
CodePipeline and AWS OpsWorks Stacks integration is not supported for deploying to Chef 11\.4 and older stacks\.

**Topics**
+ [AWS CodePipeline with AWS OpsWorks Stacks \- Chef 12 Stacks](other-services-cp-chef12.md)
+ [AWS CodePipeline with AWS OpsWorks Stacks \- Chef 11 Stacks](other-services-cp-chef11.md)