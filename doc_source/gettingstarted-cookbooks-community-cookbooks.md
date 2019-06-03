# Step 16: Update the Cookbook to Use Community Cookbooks<a name="gettingstarted-cookbooks-community-cookbooks"></a>

Finally, update the cookbook to use functionality provided in an external cookbook provided by the Chef community\. The external cookbook that you will use for this walkthrough is available through the [Chef Supermarket](https://supermarket.chef.io/), a popular location for accessing external Chef cookbooks\. This external cookbook provides a custom resource that lets you download and install applications, similar to what you did in [Step 4: Update the Cookbook to Install a Package](gettingstarted-cookbooks-install-package.md)\. However, this resource can install web applications and other application types in addition to packages\.

When a cookbook depends on another cookbook, you must specify a dependency on the other cookbook\. To declare and manage cookbook dependencies, we recommend that you use a tool called Berkshelf\. For more information about how to install Berkshelf on your local workstation, see [About Berkshelf](https://docs.chef.io/berkshelf.html) on the Chef website\.

After you install Berkshelf, follow these procedures to declare the cookbook dependency and then create a recipe that calls the resource in the external cookbook:

**To declare the cookbook dependency**

1. On your local workstation, in the `opsworks_cookbook_demo` directory, add the following line at the end of the `metadata.rb` file:

   ```
   depends "application", "5.0.0"
   ```

   This declares a dependency on a cookbook named `application`, version 5\.0\.0\. 

1. From the root of the `opsworks_cookbook_demo` directory, run the following command\. The period at the end of the command is intentional\.

   ```
   berks init .
   ```

   Berkshelf creates a number of folders and files that you can use later for more advanced scenarios\. The only file that we need for this walkthrough is the file named `Berksfile`\.

1. Add the following line at the end of the `Berksfile` file: 

   ```
   cookbook "application", "5.0.0"
   ```

   This informs Berkshelf that you want to use [application cookbook version 5\.0\.0](https://supermarket.chef.io/cookbooks/application/versions/5.0.0), which Berkshelf downloads from the Chef Supermarket\.

1. At the terminal or command prompt, run the following command from the root of the `opsworks_cookbook_demo` directory:

   ```
   berks install
   ```

   Berkshelf creates a list of the dependencies for both your cookbook and the application cookbook\. Berkshelf uses this list of dependencies in the next procedure\.

**To update the cookbook on the instance and to run the new recipe**

1. In the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `dependencies_demo.rb` that contains the following code:

   ```
   application "Install NetHack" do
     package "nethack.x86_64"
   end
   ```

   This recipe depends on the application resource from the application cookbook to install the popular text\-based adventure game NetHack on the instance\. \(You can, of course, subtitute any other package name you want, provided the package is readily available to the package manager on the instance\.\)

1. From the root of the `opsworks_cookbook_demo` directory, run the following command:

   ```
   berks package
   ```

   Berkshelf uses the list of dependencies from the previous procedure to create a file named `cookbooks-timestamp.tar.gz`, which contains the `opsworks_cookbook_demo` directory and its updated contents, including the cookbook's dependent cookbooks\. Rename this file `opsworks_cookbook_demo.tar.gz`\. 

1. Upload the updated, renamed `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::dependencies\_demo**\.

1. After you run the recipe, you should be able to log in to the instance and then type **nethack** at the command prompt to begin playing\. \(For more information about the game, see [NetHack](https://en.wikipedia.org/wiki/NetHack) and the [NetHack Guidebook](http://www.nethack.org/v343/Guidebook.html)\.\) 

In the [next step](gettingstarted-cookbooks-clean-up.md), you can clean up the AWS resources that you used for this walkthrough\. This next step is optional\. You may want to keep using these AWS resources as you continue to learn more about AWS OpsWorks Stacks\. However, keeping these AWS resources around may result in some ongoing charges to your AWS account\. If you want to keep these AWS resources around for later use, you have now completed this walkthrough, and you can skip ahead to [Next Steps](gettingstarted-cookbooks-next-steps.md)\.