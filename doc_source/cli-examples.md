# Using the AWS OpsWorks Stacks CLI<a name="cli-examples"></a>

The AWS OpsWorks Stacks command line interface \(CLI\) provides the same functionality as the console and can be used for a variety of tasks\. The AWS OpsWorks Stacks CLI is part of the AWS CLI\. For more information, including how to install and configure the AWS CLI, go to [What Is the AWS Command Line Interface?](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\. For a complete description of each command, go to the [AWS OpsWorks Stacks reference](http://docs.aws.amazon.com/cli/latest/reference/opsworks/index.html)\. 

**Note**  
If you are using a Windows\-based workstation, you can also run the AWS Tools for Windows PowerShell to perform AWS OpsWorks Stacks operations from the command line\. For more information, see [AWS Tools for Windows PowerShell](http://aws.amazon.com/documentation/powershell/)\. 

AWS OpsWorks Stacks commands have the following general format:

```
aws opsworks --region us-west-1 opsworks command-name [--argument1 value] [...]
```

If an argument value is a JSON object, you should escape the `"` characters or the command might return an error that the JSON is not valid\. For example, if the JSON object is `"{"somekey":"somevalue"}"`, you should format it as `"{\"somekey\":\"somevalue\"}"`\. An alternative approach is to put the JSON object in a file and use `file://` to include it in the command line\. The following example creates an app using an application source object stored in appsource\.json\.

```
aws opsworks --region us-west-1 create-app --stack-id 8c428b08-a1a1-46ce-a5f8-feddc43771b8 --name SimpleJSP --type java --app-source file://appsource.json 
```

Most commands return one or more values, packaged as a JSON object\. The following sections contain some examples\. For a detailed description of the return values for each command, go to the [AWS OpsWorks Stacks reference](http://docs.aws.amazon.com/cli/latest/reference/opsworks/index.html)\.

**Note**  
AWS CLI commands must specify a region, as shown in the examples\. Valid values for the \-\-region parameter are shown in the following table\. To simplify your AWS OpsWorks Stacks command strings, configure the CLI to specify your default region, so you can omit the `--region` parameter\. If you typically work in multiple regional endpoints, do not configure the AWS CLI to use a default regional endpoint\. The Canada \(Central\) Region endpoint is available in the API and AWS CLI only; it is not available for stacks that you create in the AWS Management Console\. For more information, see [Configuring the AWS Region](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-installing-specifying-region)\.  


| Region name | Command code | 
| --- | --- | 
| US East \(Ohio\) Region | us\-east\-2 | 
| US East \(N\. Virginia\) Region | us\-east\-1 | 
| US West \(N\. California\) Region | us\-west\-1 | 
| US West \(Oregon\) Region | us\-west\-2 | 
| Canada \(Central\) Region | ca\-central\-1 | 
| EU \(Ireland\) Region | eu\-west\-1 | 
| EU \(London\) Region | eu\-west\-2 | 
| EU \(Paris\) Region | eu\-west\-3 | 
| EU \(Frankfurt\) Region | eu\-central\-1 | 
| Asia Pacific \(Tokyo\) Region | ap\-northeast\-1 | 
| Asia Pacific \(Seoul\) Region | ap\-northeast\-2 | 
| Asia Pacific \(Mumbai\) Region | ap\-south\-1 | 
| Asia Pacific \(Singapore\) Region | ap\-southeast\-1 | 
| Asia Pacific \(Sydney\) Region | ap\-southeast\-2 | 
| South America \(São Paulo\) Region | sa\-east\-1 | 

To use a CLI command, you must have the appropriate permissions\. For more information on AWS OpsWorks Stacks permissions, see [Managing User Permissions](opsworks-security-users.md)\. To determine the permissions required for a particular command, see the command's reference page in the [AWS OpsWorks Stacks reference](http://docs.aws.amazon.com/cli/latest/reference/opsworks/index.html)\.

The following sections describe how to use the AWS OpsWorks Stacks CLI to perform a variety of common tasks\. 