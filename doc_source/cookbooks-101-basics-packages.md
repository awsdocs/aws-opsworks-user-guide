# Example 1: Installing Packages<a name="cookbooks-101-basics-packages"></a>

Package installation is one of the more common uses of recipes and can be quite simple, depending on the package\. For example, the following recipe installs Git on a Linux system\.

```
package 'git' do
  action :install
end
```

The [`package` resource](https://docs.chef.io/chef/resources.html#package) handles package installation\. For this example, you don't need to specify any attributes\. The resource name is the default value for the `package_name` attribute, which identifies the package\. The `install` action directs the provider to install the package\. You could make the code even simpler by skipping `install`; it's the `package` resource's default action\. When you run the recipe, Chef uses the appropriate provider to install the package\. On the Ubuntu system that you will use for the example, the provider installs Git by calling `apt-get`\.

**Note**  
Installing software on a Windows system requires a somewhat different procedure\. For more information, see [Installing Windows Software](cookbooks-101-opsworks-install-software.md)\.

To use Test Kitchen to run this recipe in Vagrant, you first need to set up a cookbook and initialize and configure Test Kitchen\. The following is for a Linux system, but the procedure is essentially similar for Windows and Macintosh systems\. Start by opening a Terminal window; all of the examples in this chapter use command\-line tools\.

**To prepare the cookbook**

1. In your home directory, create a subdirectory named `opsworks_cookbooks`, which will contain all the cookbooks for this chapter\. Then create a subdirectory for this cookbook named `installpkg` and navigate to it\.

1. In `installpkg`, create a file named `metadata.rb` that contains the following code\.

   ```
   name "installpkg"
   version "0.1.0"
   ```

   For simplicity, the examples in this chapter just specify the cookbook name and version, but `metadata.rb` can contain a variety of cookbook metadata\. For more information, see [About Cookbook Metadata](http://docs.chef.io/cookbook_repo.html#about-cookbook-metadata)\.
**Note**  
Make sure to create `metadata.rb` before you initialize Test Kitchen; it uses the data to create the default configuration file\.

1. In `installpkg`, run `kitchen init`, which initializes Test Kitchen and installs the default Vagrant driver\.

1. The `kitchen init` command creates a YAML configuration file in `installpkg` named `.kitchen.yml`\. Open the file in your favorite text editor\. The `.kitchen.yml` file includes a `platforms` section that specifies which systems to run the recipes on\. Test Kitchen creates an instance and runs the specified recipes on each platform\. 
**Note**  
By default, Test Kitchen runs recipes one platform at a time\. If you add a `-p` argument to any command that creates an instance, Test Kitchen will run the recipes on every platform, in parallel\.

   A single platform is sufficient for this example, so edit `.kitchen.yml` to remove the `centos-6.4` platform\. Your `.kitchen.yml` file should now look like this:

   ```
   ---
   driver:
     name: vagrant
   
   provisioner:
     name: chef_solo
   
   platforms:
     - name: ubuntu-12.04
   
   suites:
     - name: default
       run_list:
         - recipe[installpkg::default]
       attributes:
   ```

   Test Kitchen runs only those recipes that are in the `.kitchen.yml` run list\. You identify recipes by using the `[cookbook_name::recipe_name]` format, where *recipe\_name* omits the `.rb` extension\. Initially, the `.kitchen.yml` run list contains the cookbook's default recipe, `installpkg::default`\. That's the recipe that you are going to implement, so you don't need to modify the run list\.

1. Create a subdirectory of `installpkg` named `recipes`\.

   If a cookbook contains recipes—most do—they must be in the `recipes` subdirectory\.

You can now add the recipe to the cookbook and use Test Kitchen to run it on an instance\.

**To run the recipe**

1. Create a file named `default.rb` that contains the Git installation example code from the beginning of the section and save it to the `recipes` subdirectory\.

1. In the `installpkg` directory, run `kitchen converge`\. This command starts a new Ubuntu 12\.04 LTS instance in Vagrant, copies your cookbooks to the instance, and initiates a Chef run to execute the recipes in the `.kitchen.yml` run list\.

1. To verify that the recipe was successful, run `kitchen login`, which opens an SSH connection to the instance\. Then run `git --version` to verify that Git was successfully installed\. To return to your workstation, run `exit`\.

1. When you are finished, run `kitchen destroy` to shut down the instance\. The next example uses a different cookbook\.

This example was a good way to get started, but it is especially simple\. Other packages can be more complicated to install; you might need to do any or all of the following:
+ Create and configure a user\.
+ Create one or more directories for data, logs, and so on\.
+ Install one or more configuration files\.
+ Specify a different package name or attribute values for different operating systems\.
+ Start a service and then restart it as needed\.

The following examples describe how to address these issues, along with some other useful operations\.