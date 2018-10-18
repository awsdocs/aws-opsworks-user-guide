# Step 7: Update the Cookbook to Create a Directory<a name="gettingstarted-cookbooks-create-directory"></a>

Update your cookbook by adding a recipe that adds a directory to the instance\. This is similar to running the Linux mkdir command or the Windows md or mkdir commands\.

**To update the cookbook on the instance and to run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `create_directory.rb` with the following code\. For more information, go to [directory](https://docs.chef.io/resource_directory.html): 

   ```
   directory "Create a directory" do
     group "root"
     mode "0755"
     owner "ec2-user"
     path "/tmp/create-directory-demo"  
   end
   ```

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::create\_directory**\.

**To test the recipe**

1. Log in to the instance, if you have not done so already\.

1. From the command prompt, run the following command to confirm that the new directory was added:

   ```
   ls -la /tmp/create-directory-demo
   ```

   Information about the newly\-added directory is displayed, including information such as permissions, owner name, and group name: 

   ```
   drwxr-xr-x 2 ec2-user root 4096 Nov 18 00:35 .
   drwxrwxrwt 6 root     root 4096 Nov 24 18:17 ..
   ```

In the [next step](gettingstarted-cookbooks-create-file.md), you will update the cookbook to create a file on the instance\.