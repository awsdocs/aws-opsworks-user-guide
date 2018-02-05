# Per\-layer Operating System Package Installations<a name="per-layer-os-package-install"></a>

Starting with Chef 12, you must use custom recipes to install packages on layers that are running different operating systems\. This approach provides you with maximum flexibility and control over package installations\. 

For example, suppose that you want to install Apache on layers that are running RedHat, Ubuntu, and Amazon versions of the Linux operating system\. The Apache package for RedHat and Amazon Linux is called `httpd`, but on Ubuntu, it is called `apache2`\. 

To address the difference in package naming, you can use syntax similar to that in the following example recipe\. The recipe installs the Apache package appropriate for each operating system\. This example is based on the [Chef documentation](https://docs.chef.io/)\. 

```
package "Install Apache" do
   case node[:platform]
      when "redhat", "amazon"
         package_name "httpd"
      when "ubuntu"
         package_name "apache2"
   end
end
```

For detailed information on how to use the `package` resource to manage packages, go to the [package](https://docs.chef.io/resource_package.html) page in the Chef documentation\. 

Alternatively, you can use the `value_for_platform` helper method from the Chef Recipe DSL \(domain\-specific language\), which accomplishes the same thing more succinctly: 

```
package "Install Apache" do
   package_name value_for_platform(
      ["redhat", "amazon"] => { "default" => "httpd" },
      ["ubuntu"] => { "default" => "apache2" }
   )
end
```

For information on using the `value_for_platform` helper method, go to [About the Recipe DSL](https://docs.chef.io/dsl_recipe.html)\. 