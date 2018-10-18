# deploy Attributes<a name="attributes-recipes-deploy"></a>

The [built\-in deploy cookbook's `deploy.rb` attributes file](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/deploy/attributes/deploy.rb) defines the following attributes in the `opsworks` namespace\. For more information on deploy directories, see [Deploy Recipes](create-custom-deploy.md)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.

**deploy\_keep\_releases **  <a name="attributes-recipes-deploy-global-keep-releases"></a>
A global setting for the number of app deployments that AWS OpsWorks Stacks will store \(number\)\. The default value is 5\. This value controls the number of times you can roll back an app\.  

```
node[:opsworks][:deploy_keep_releases]
```

**group **  
\(Linux only\) The `group` setting for the app's deploy directory \(string\)\. The default value depends on the instance's operating system:  
+ For Ubuntu instances, the default value is `www-data`\.
+ For Amazon Linux or RHEL instances that are members of a Rails App Server layer that uses Nginx and Unicorn, the default value is `nginx`\.
+ For all other Amazon Linux or RHEL instances, the default value is `apache`\.

```
node[:opsworks][:deploy_user][:group]
```

**user **  
\(Linux only\) The `user` setting for the app's deploy directory \(string\)\. The default value is `deploy`\.  

```
node[:opsworks][:deploy_user][:user]
```