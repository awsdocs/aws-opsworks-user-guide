# AWS OpsWorks Stacks Permissions Levels<a name="opsworks-security-users-standard"></a>

This section lists the actions that are allowed by the **Show**, **Deploy**, and **Manage** permissions levels on the AWS OpsWorks Stacks **Permissions** page\. It also includes a list of actions that you can grant permissions only by attaching an IAM policy to the user\.

**Show**  
The **Show** level allows `DescribeXYZ` commands, with the following exceptions:  

```
DescribePermissions
DescribeUserProfiles
DescribeMyUserProfile
DescribeStackProvisioningParameters
```
If an administrative user has enabled self\-management for the user, **Show** users can also use `DescribeMyUserProfile` and `UpdateMyUserProfile`\. For more information on self management, see [Editing User Settings](opsworks-security-users-manage-edit.md)\. 

**Deploy**  
The following actions are allowed by the **Deploy** level, in addition to the actions allowed by the **Show** level\.  

```
CreateDeployment
UpdateApp
```

**Manage**  
The following actions are allowed by the **Manage** level, in addition to the actions allowed by the **Deploy** and **Show** levels\.  

```
AssignInstance
AssignVolume
AssociateElasticIp
AttachElasticLoadBalancer
CreateApp
CreateInstance
CreateLayer
DeleteApp
DeleteInstance
DeleteLayer
DeleteStack
DeregisterElasticIp
DeregisterInstance
DeregisterRdsDbInstance
DeregisterVolume
DescribePermissions
DetachElasticLoadBalancer
DisassociateElasticIp
GrantAccess
GetHostnameSuggestion
RebootInstance
RegisterElasticIp
RegisterInstance
RegisterRdsDbInstance
RegisterVolume
SetLoadBasedAutoScaling
SetPermission
SetTimeBasedAutoScaling
StartInstance
StartStack
StopInstance
StopStack
UnassignVolume
UpdateElasticIp
UpdateInstance
UpdateLayer
UpdateRdsDbInstance
UpdateStack
UpdateVolume
```

**Permissions That Require an IAM Policy**  
You must grant permissions for the following actions by attaching an appropriate IAM policy to the user\. For some examples, see [Example Policies](opsworks-security-users-examples.md)\.  

```
CloneStack
CreateStack
CreateUserProfile
DeleteUserProfile
DescribeUserProfiles
UpdateUserProfile
```