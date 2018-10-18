# Step 6: Update the Cookbook to Add a User<a name="gettingstarted-cookbooks-add-user"></a>

Update your cookbook by adding a recipe that adds a local user to the instance and sets the user's home directory and shell\. This is similar to running the Linux adduser or useradd commands or the Windows net user command\. You add a local user to an instance, for example, when you want to control access to the instance's files and directories\.

You can also manage users without using cookbooks\. For more information, see [Managing Users](opsworks-security-users-manage.md)\.

**To update the cookbook on the instance and to run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `add_user.rb` with the following code \(for more information, go to [user](https://docs.chef.io/resource_user.html)\): 

   ```
   user "Add a user" do
     home "/home/jdoe"
     shell "/bin/bash"
     username "jdoe"  
   end
   ```

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::add\_user**\.

**To test the recipe**

1. Log in to the instance, if you have not done so already\.

1. From the command prompt, run the following command to confirm that the new user was added:

   ```
   grep jdoe /etc/passwd
   ```

   Information similar to the following is displayed about the user, including details such as the user's name, ID number, group ID number, home directory, and shell:

   ```
   jdoe:x:501:502::/home/jdoe:/bin/bash
   ```

In the [next step](gettingstarted-cookbooks-create-directory.md), you will update the cookbook to create a directory on the instance\.