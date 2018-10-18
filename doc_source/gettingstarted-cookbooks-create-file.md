# Step 8: Update the Cookbook to Create and Copy Files<a name="gettingstarted-cookbooks-create-file"></a>

Update your cookbook by adding a recipe that adds two files to the instance\. The first resource in the recipe creates a file completely with recipe code\. This is similar to running the Linux cat, echo, or touch commands or the Windows echo or fsutil commands\. This technique is useful for a few, small, or simple files\. The second resource in the recipe copies a file in the cookbook to another directory on the instance\. This is similar to running the Linux cp command or the Windows copy command\.This technique is useful for many, large, or complex files\.

Before you start this step, complete [Step 7: Update the Cookbook to Create a Directory](gettingstarted-cookbooks-create-directory.md) to make sure that the files' parent directory already exists\.

**To update the cookbook on the instance and run the new recipe**

1. On your local workstation, in the `opsworks_cookbook_demo` directory, create a subdirectory named `files`\. 

1. In the `files` subdirectory, create a file named `hello.txt` with the following text: **Hello, World\!** 

1. In the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `create_files.rb` with the following code\. For more information, go to [file](https://docs.chef.io/resource_file.html) and [cookbook\_file](https://docs.chef.io/resource_cookbook_file.html)\.

   ```
   file "Create a file" do
     content "<html>This is a placeholder for the home page.</html>"
     group "root"
     mode "0755"
     owner "ec2-user"
     path "/tmp/create-directory-demo/index.html"
   end
   
   cookbook_file "Copy a file" do  
     group "root"
     mode "0755"
     owner "ec2-user"
     path "/tmp/create-directory-demo/hello.txt"
     source "hello.txt"  
   end
   ```

   The `file` resource creates a file in the specified path\. The `cookbook_file` resource copies the file from the `files` directory that you just created in the cookbook \(Chef expects to find a standard\-named subdirectory named `files` that it can copy files from\) to another directory on the instance\.

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::create\_files**\.

**To test the recipe**

1. Log in to the instance, if you have not done so already\.

1. From the command prompt, run the following commands, one at a time, to confirm that the new files were added:

   ```
   sudo cat /tmp/create-directory-demo/index.html
   
   sudo cat /tmp/create-directory-demo/hello.txt
   ```

   The files' contents are displayed:

   ```
   <html>This is a placeholder for the home page.</html>
   
   Hello, World!
   ```

In the [next step](gettingstarted-cookbooks-run-command.md), you will update the cookbook to run a command on the instance\.