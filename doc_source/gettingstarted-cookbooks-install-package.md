# Step 4: Update the Cookbook to Install a Package<a name="gettingstarted-cookbooks-install-package"></a>

Update your cookbook by adding a recipe that installs on the instance a package that contains the popular text editor GNU Emacs\.

Although you can just as easily log in to the instance and install the package once, writing a recipe enables you to run the recipe from AWS OpsWorks Stacks once to install multiple packages on multiple instances in a stack simultaneously\. 

**To update the cookbook to install a package**

1. Back on your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `install_package.rb` with the following code: 

   ```
   package "Install Emacs" do
     package_name "emacs"
   end
   ```

   This recipe installs the `emacs` package on the instance\. \(For more information, go to [package](https://docs.chef.io/resource_package.html)\.\)
**Note**  
You can give a recipe any file name you want\. Just be sure to specify the correct recipe name whenever you want AWS OpsWorks Stacks to run the recipe\.

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

This new recipe runs when you update the cookbook on the instance and then run the new recipe from within the updated cookbook\. The next step describes how to do this\. 

After you complete the [next step](gettingstarted-cookbooks-copy-cookbook.md), you will be able to log in to the instance and then type emacs from the command prompt to launch GNU Emacs\. \(For more information, see [Connect to Your Linux Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html)\.\) To exit GNU Emacs, press **Ctrl\+X**, then **Ctrl\+C**\.

**Important**  
To be able to log in to the instance, you must first provide AWS OpsWorks Stacks with information about your public SSH key \(which you can create with tools such as ssh\-keygen or PuTTYgen\), and then you must set permissions on the `MyCookbooksDemoStack` stack to enable your IAM user to log in to the instance\. For instructions, see [Registering an IAM User's Public SSH Key](security-settingsshkey.md) and [Logging In with SSH](workinginstances-ssh.md)\.