# ruby Attributes<a name="attributes-recipes-ruby"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`ruby` attributes](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/ruby/attributes/ruby.rb) specify the Ruby version that is used by applications\. Note that attribute usage changed with the introduction of semantic versioning in Ruby 2\.1\. For more information about how to specify a version, including examples, see [Ruby Versions](workingcookbook-ruby.md)\. For complete details on how AWS OpsWorks Stacks determines the Ruby version, see the built\-in attributes file, [ruby\.rb](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/ruby/attributes/ruby.rb)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.

**full\_version**  <a name="attributes-recipes-ruby-full"></a>
The full version number \(string\)\. You should not override this attribute\. Instead, use [\[:opsworks\]\[:ruby\_version\]](attributes-json-opsworks-other.md#attributes-json-opsworks-ruby-version) and the appropriate patch version attribute to specify a version\.  

```
[:ruby][:full_version]
```

**major\_version**  <a name="attributes-recipes-ruby-major"></a>
The major version number \(string\)\. You should not override this attribute\. Instead, use [\[:opsworks\]\[:ruby\_version\]](attributes-json-opsworks-other.md#attributes-json-opsworks-ruby-version) to specify the major version\.  

```
[:ruby][:major_version]
```

**minor\_version**  <a name="attributes-recipes-ruby-minor"></a>
The minor version number \(string\)\. You should not override this attribute\. Instead, use [\[:opsworks\]\[:ruby\_version\]](attributes-json-opsworks-other.md#attributes-json-opsworks-ruby-version) to specify the minor version\.  

```
[:ruby][:minor_version]
```

**patch**  <a name="attributes-recipes-ruby-patch"></a>
The patch level \(string\)\. This attribute is valid for Ruby version 2\.0\.0 and earlier\. For later Ruby versions, use the `patch_version` attribute\.  

```
[:ruby][:patch]
```
The patch number must be prefaced by `p`\. For example, you would use the following custom JSON to specify patch level 484\.  

```
{
  "ruby":{"patch":"p484"}
}
```

**patch\_version**  <a name="attributes-recipes-ruby-patch-version"></a>
The patch number \(string\)\. This attribute is valid for Ruby version 2\.1 and later\. For earlier Ruby versions, use the `patch` attribute\.  

```
[:ruby][:patch_version]
```

**pkgrelease**  <a name="attributes-recipes-ruby-pkgrelease"></a>
The package release number \(string\)\.  

```
[:ruby][:pkgrelease]
```