# Step 10: Update the Cookbook to Run a Script<a name="gettingstarted-cookbooks-run-script"></a>

Update your cookbook by adding a recipe that runs a script on the instance\. This recipe creates a directory and then creates a file in that directory\. Writing a recipe to run a script that contains multiple commands is easier than running those commands one at a time\.

**To update the cookbook on the instance and run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `run_script.rb` with the following code\. For more information, go to [script](https://docs.chef.io/resource_script.html)\. 

   ```
   script "Run a script" do
     interpreter "bash"
     code <<-EOH
       mkdir -m 777 /tmp/run-script-demo
       touch /tmp/run-script-demo/helloworld.txt
       echo "Hello, World!" > /tmp/run-script-demo/helloworld.txt
     EOH
   end
   ```

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::run\_script**\.

**To test the recipe**

1. Log in to the instance, if you have not done so already\.

1. From the command prompt, run the following command to confirm that the new file was added:

   ```
   sudo cat /tmp/run-script-demo/helloworld.txt
   ```

   The file's contents are displayed:

   ```
   Hello, World!
   ```

In the [next step](gettingstarted-cookbooks-manage-service.md), you will update the cookbook to manage a service on the instance\.