# Common Debugging and Troubleshooting Issues<a name="common-issues"></a>

This section describes some commonly encountered debugging and troubleshooting issues and their solutions\.

**Topics**
+ [Troubleshooting AWS OpsWorks Stacks](common-issues-troubleshoot.md)
+ [Troubleshooting Instance Registration](#common-issues-instance-registration)

## Troubleshooting Instance Registration<a name="common-issues-instance-registration"></a>

This section contains some commonly encountered instance registration issues and their solutions\.

**Note**  
If you are having registration problems, run `register` with the `--debug` argument, which provides additional debugging information\.

**Topics**
+ [EC2User Is Not Authorized to Perform: \.\.\.](#common-issues-instance-registration-ec2user)
+ [Credential Should Be Scoped to a Valid Region](#common-issues-instance-registration-valid-region)

### EC2User Is Not Authorized to Perform: \.\.\.<a name="common-issues-instance-registration-ec2user"></a>

**Problem:** A `register` command returns something like the following:

```
A client error (AccessDenied) occurred when calling the CreateGroup operation: 
User: arn:aws:iam::123456789012:user/ImportEC2User is not authorized to
perform: iam:CreateGroup on resource: 
arn:aws:iam::123456789012:group/AWS/OpsWorks/OpsWorks-b583ce55-1d01-4695-b3e5-ee19257d1911
```

**Cause:** The `register` command is running with IAM user credentials that do not grant the required permissions\. The user's policy must allow the `iam:CreateGroup` action, among others\.

**Solution** Provide `register` with IAM user credentials that have the required permissions\. For more information, see [Installing and Configuring the AWS CLI](registered-instances-register-registering-cli.md)\.

### Credential Should Be Scoped to a Valid Region<a name="common-issues-instance-registration-valid-region"></a>

**Problem:** A `register` command returns the following:

```
A client error (InvalidSignatureException) occurred when calling the
DescribeStacks operation: Credential should be scoped to a valid region, not 'cn-north-1'.
```

**Cause:** The command's region must be a valid AWS OpsWorks Stacks region\. For a list of supported regions, see [Region Support](gettingstarted_intro.md#gettingstarted-intro-region)\. This error typically occurs for one of the following reasons:
+ The stack is in a different region, and you assigned a the stack's region to the command's `--region` argument\.

  You don't need to specify a stack region; AWS OpsWorks Stacks automatically determines it from the stack ID\.
+ You omitted `--region` argument, which implicitly specifies the default region, but your default region is not supported by AWS OpsWorks Stacks\.

**Solution:** Explicitly set `--region` to a supported AWS OpsWorks Stacks region``, or edit your AWS CLI `config` file to change the default region to a supported AWS OpsWorks Stacks region\. For more information, see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.