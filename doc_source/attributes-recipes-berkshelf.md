# opsworks\_berkshelf Attributes<a name="attributes-recipes-berkshelf"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`opsworks_berkshelf` attributes](https://github.com/aws/opsworks-cookbooks/blob/master-chef-11.10/opsworks_berkshelf/attributes/default.rb) specify the Berkshelf configuration\. For more information, see [Berkshelf](http://berkshelf.com/)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.

**debug**  <a name="attributes-recipes-berkshelf-debug"></a>
Whether to include Berkshelf debugging information in the Chef log \(Boolean\)\. The default value is `false`\.  

```
node['opsworks_berkshelf]['debug']
```