# Step 15: Update the Cookbook to Use Conditional Logic<a name="gettingstarted-cookbooks-conditional-logic"></a>

Now update your cookbook by adding a recipe that uses *conditional logic*, a technique that runs code only if certain conditions are met\. For more information, go to [if Statements](https://docs.chef.io/dsl_recipe.html#if-statements) and [case Statements](https://docs.chef.io/dsl_recipe.html#case-statements)\.

This recipe does two things based on data bag content: displays a message in the log identifying the operating system that the instance is running on and, only if the operating system is Linux, installs a package by using the correct package manager for the given Linux distribution\. This package is named tree; it is a simple app for visualizing directory lists\.

**To update the cookbook on the instance and to run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo directory`, create a file named `conditional_logic.rb` that contains the following code:

   ```
   instance = search("aws_opsworks_instance").first
   os = instance["os"]
   
   if os == "Red Hat Enterprise Linux 7"
     Chef::Log.info("********** Operating system is Red Hat Enterprise Linux. **********")
   elsif os == "Ubuntu 12.04 LTS" || os == "Ubuntu 14.04 LTS" || os == "Ubuntu 16.04 LTS" || os == "Ubuntu 18.04 LTS"
     Chef::Log.info("********** Operating system is Ubuntu. **********") 
   elsif os == "Microsoft Windows Server 2012 R2 Base"
     Chef::Log.info("********** Operating system is Windows. **********")
   elsif os == "Amazon Linux 2015.03" || os == "Amazon Linux 2015.09" || os == "Amazon Linux 2016.03" || os == "Amazon Linux 2016.09" || os == "Amazon Linux 2017.03" || os == "Amazon Linux 2017.09" || os == "Amazon Linux 2018.03" || os == "Amazon Linux 2"
     Chef::Log.info("********** Operating system is Amazon Linux. **********")
   elsif os == "CentOS Linux 7"
     Chef::Log.info("********** Operating system is CentOS 7. **********")
   else
     Chef::Log.info("********** Cannot determine operating system. **********")
   end
   
   case os
   when "Ubuntu 12.04 LTS", "Ubuntu 14.04 LTS", "Ubuntu 16.04 LTS", "Ubuntu 18.04 LTS"
     apt_package "Install a package with apt-get" do
       package_name "tree"
     end
   when "Amazon Linux 2015.03", "Amazon Linux 2015.09", "Amazon Linux 2016.03", "Amazon Linux 2016.09", "Amazon Linux 2017.03", "Amazon Linux 2017.09", "Amazon Linux 2018.03", "Amazon Linux 2", "Red Hat Enterprise Linux 7", "CentOS Linux 7"
     yum_package "Install a package with yum" do
       package_name "tree"
     end
   else
     Chef::Log.info("********** Cannot determine operating system type, or operating system is not Linux. Package not installed. **********")
   end
   ```

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::conditional\_logic**\. 

**To test the recipe**

1. With the **Running command execute\_recipes** page displayed from the previous procedures, for **cookbooks\-demo1**, for **Log**, choose **show**\. The **execute\_recipes** log page is displayed\.

1. Scroll down through the log and find an entry that looks similar to the following:

   ```
   [2015-11-16T19:59:05+00:00] INFO: ********** Operating system is Amazon Linux. **********
   ```

   Because the instance's operating system is Amazon Linux 2016\.09, only the preceding entry \(of the five possible entries in the recipe's code\) will be displayed in the log\. 

1. If the operating system is Linux, the recipe installs the tree package\. To see a visualization of a directory's contents, type **tree** at the command prompt from the desired directory or with the desired directory's path \(for example, `tree /var/chef/runs`\)\.

In the [next step](gettingstarted-cookbooks-community-cookbooks.md), you will update the cookbook to use functionality from an external cookbook provided by the Chef community\.