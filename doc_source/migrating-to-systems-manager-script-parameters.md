# Step 3: Set up your environment to run the script<a name="migrating-to-systems-manager-script-parameters"></a>

Set up your environment to the run the script by using the following command\.

```
pipenv install -r requirements.txt
pipenv shell
```

**Note**  
 Currently, the script can only provision single\-layer applications in Application Manager\. For example, if you run the script twice for two layers in the same stack, the script creates two different applications in Application Manager\. 

After setting up your environment, review the script parameters\. You can view the available options for the migration script by running the `python3 stack_exporter.py --help` command\.


****  

| Parameter | Description | Required | Type | Default value | 
| --- | --- | --- | --- | --- | 
| \-\-layer\-id | Exports a CloudFormation template for this OpsWorks layer ID\. | Yes | string |  | 
| \-\-region | The AWS Region for the OpsWorks stack\. If your OpsWorks stack Region and API endpoint Region are different, use the stack Region\. This is the same Region as the other resources part of your OpsWorks stack \(for example, EC2 instances and subnets\)\. | No | string | us\-east\-1 | 
| \-\-provision\-application | By default, the script provisions the application exported by the CloudFormation template\. Pass this parameter into the script with a value of FALSE to skip provisioning of the CloudFormation template\. | No | Boolean | TRUE | 
| \-\-launch\-template | This parameter defines whether to use an existing launch template, or create a new launch template\. You can create a new launch template that uses the recommended instance properties, or that uses instance properties that match an online instance\.  Valid values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/migrating-to-systems-manager-script-parameters.html)  | No | string | RECOMMENDED | 
| \-\-system\-updates |  Defines whether to perform kernel and package updates when the instance boots\. Valid values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/migrating-to-systems-manager-script-parameters.html)  | No | string | ALL\_UPDATES | 
| \-\-http\-username | The name of the Systems Manager SecureString parameter which stores the user name used to authenticate to the HTTP archive that contains the custom cookbooks\. | No | string |  | 
| \-\-http\-password | The name of the Systems Manager SecureString parameter which stores the password used to authenticate to the HTTP archive that contains the custom cookbooks\. | No | string |  | 
| \-\-repo\-private\-key | The name of the Systems Manager SecureString parameter which stores the SSH key used to authenticate to the repository that contains the custom cookbooks\. If the repository is on GitHub, you must generate a new Ed25519 SSH key\. If you do not generate a new Ed25519 SSH key, the connection to the GitHub repository fails\. | No | string |  | 
| \-\-lb\-type | The type of load balancer, if any, to create when migrating your existing load balancer\. Valid values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/migrating-to-systems-manager-script-parameters.html)  | No | string | ALB | 
| \-\-lb\-access\-logs\-path | The path to an existing S3 bucket and prefix for storing the load balancer access logs\. The S3 bucket and load balancer must be in the same Region\. If you do not provide a value and the \-\-lb\-type parameter value is set to None, the script creates a new S3 bucket and prefix\. Be sure there is an appropriate bucket policy for this prefix\.  | No | string |  | 
| \-\-enable\-instance\-protection | If set to TRUE , the script creates a custom termination policy \(Lambda function\) for your Auto Scaling group\. EC2 instances with a protected\_instance tag are protected from scale\-in events\. Add a protected\_instance tag to each EC2 instance that you want to protect from scale\-in events\. | No | Boolean | FALSE | 
| \-\-command\-logs\-bucket | The name of an existing S3 bucket to store the AWSApplyChefRecipe and MountEBSVolumes logs\. If you do not provide a value, the script creates a new S3 bucket\. | No | string | aws\-opsworks\-application\-manager\-logs\-account\-id | 
| \-\-custom\-json\-bucket | The name of an existing S3 bucket to store custom JSON\. If you do not provide a value, the script creates a new S3 bucket\. | No | string | aws\-apply\-chef\-application\-manager\-transition\-data\-account\-id | 

**Notes:**
+ If you use a private GitHub repository, you must create a new `Ed25519` host key for SSH\. This is because GitHub changed which keys are supported in SSH and removed the unencrypted Git protocol\. For more information about the `Ed25519` host key, see the GitHub blog post [Improving Git protocol security on GitHub](https://github.blog/2021-09-01-improving-git-protocol-security-github/)\. After you generate a new `Ed25519` host key, create a Systems Manager `SecureString` parameter for the SSH key and use the `SecureString` parameter name as the value for the `--repo-private-key` parameter\. For more information about how to create a Systems Manager `SecureString` parameter, see [Create a SecureString parameter \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/param-create-cli.html#param-create-cli-securestring) or [Create a Systems Manager parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-create-console.html) in the *AWS Systems Manager User Guide*\.
+ The `--http-username`, `--http-password` and `--http-password` parameters refer to the name of a Systems Manager `SecureString` parameter\. The migration script uses these parameters when you run the `AWS-ApplyChefRecipes` document\. 
+ The `--http-username` parameter requires that you also specify a value for the `--http-password` parameter\.
+  The `--http-password` parameter requires that you also specify a value for the `--http-username` parameter\. 
+ Do not set values for both `--http-password` and `--repo-private-key`\. Provide either a Systems Manager `SecureString` parameter name of an SSH key \(`--repo-private-key`\), or a repository user name \(`--http-username`\) and password \(`--http-password`\)\.