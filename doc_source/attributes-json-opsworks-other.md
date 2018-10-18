# Other Top\-level opsworks Attributes<a name="attributes-json-opsworks-other"></a>

This section contains the `opsworks` attributes that do not have child attributes\.

**activity**  <a name="attributes-json-opsworks-activity"></a>
The activity that is associated with the attributes, such as `deploy` \(string\)\.  

```
node["opsworks"]["activity"]
```

**agent\_version**  <a name="attributes-json-opsworks-agent"></a>
The version of the instance's OpsWorks agent \(string\)\.  

```
node["opsworks"]["agent_version"]
```

**deploy\_chef\_provider**  
The Chef deploy provider, which influences a deployed app's directory structure \(string\)\. For more information, see [deploy](https://docs.chef.io/resource_deploy.html)\.You can set this attribute to one the following:  
+ `Branch`
+ `Revision`
+ `Timestamped` \(default value\)

```
node["opsworks"]["deploy_chef_provider"]
```

**ruby\_stack**  <a name="attributes-json-opsworks-ruby-stack"></a>
The Ruby stack \(string\)\. The default setting is the enterprise version \(`ruby_enterprise`\)\. For the MRI version, set this attribute to `ruby`\.  

```
node["opsworks"]["ruby_stack"]
```

**ruby\_version**  <a name="attributes-json-opsworks-ruby-version"></a>
The Ruby version that will be used by applications \(string\)\. You can use this attribute to specify only the major and minor version\. You must use the appropriate [\["ruby"\]](attributes-recipes-ruby.md) attribute to specify the patch version\. For more information about how to specify a version, including examples, see [Ruby Versions](workingcookbook-ruby.md)\. For complete details on how AWS OpsWorks Stacks determines the Ruby version, see the built\-in attributes file, [ruby\.rb](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/ruby/attributes/ruby.rb)\.  

```
node["opsworks"]["ruby_version"]
```

**run\_cookbook\_tests**  
Whether to run [minitest\-chef\-handler](https://github.com/calavera/minitest-chef-handler) tests on your Chef 11\.4 cookbooks \(Boolean\)\.  

```
node["opsworks"]["run_cookbook_tests"]
```

**sent\_at**  <a name="attributes-json-opsworks-sent"></a>
When this command was sent to the instance \(number\)\.  

```
node["opsworks"]["sent_at"]
```

**deployment**  <a name="attributes-json-opsworks-deployment"></a>
If these attributes are associated with a deploy activity, `deployment` is set to the deployment ID, an AWS OpsWorks Stacks\-generated GUID that uniquely identifies the deployment \(string\)\. Otherwise the attribute is set to null\.  

```
node["opsworks"]["deployment"]
```