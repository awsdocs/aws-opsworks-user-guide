# Step 11: Update the Cookbook to Manage a Service<a name="gettingstarted-cookbooks-manage-service"></a>

Update your cookbook by adding a recipe that manages a service on the instance\. This is similar to running the Linux service command or the Windows net stop, net start, and similar commands\.This recipe stops the crond service on the instance\.

**To update the cookbook on the instance and run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `manage_service.rb` with the following code\. For more information, go to [service](https://docs.chef.io/resource_service.html)\. 

   ```
   service "Manage a service" do
     action :stop
     service_name "crond"  
   end
   ```

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::manage\_service**\.

**To test the recipe**

1. Log in to the instance, if you have not done so already\.

1. From the command prompt, run the following command to confirm that the crond service is stopped:

   ```
   service crond status
   ```

   The following is displayed:

   ```
   crond is stopped
   ```

1. To restart the crond service, run the following command:

   ```
   sudo service crond start
   ```

   The following is displayed:

   ```
   Starting crond:  [  OK  ]
   ```

1.  To confirm that the crond service has started, run the following command again:

   ```
   service crond status
   ```

   Information similar to the following is displayed:

   ```
   crond (pid  3917) is running...
   ```

In the [next step](gettingstarted-cookbooks-custom-json.md), you will update the cookbook to reference information stored as custom JSON on the instance\.