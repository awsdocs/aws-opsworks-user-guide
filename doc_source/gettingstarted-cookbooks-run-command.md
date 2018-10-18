# Step 9: Update the Cookbook to Run a Command<a name="gettingstarted-cookbooks-run-command"></a>

Update your cookbook by adding a recipe that runs a command that creates an SSH key on the instance\. 

**To update the cookbook on the instance and run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `run_command.rb` with the following code\. For more information, go to [execute](https://docs.chef.io/resource_execute.html)\.

   ```
   execute "Create an SSH key" do
     command "ssh-keygen -f /tmp/my-key -N fLyC3jbY"
   end
   ```

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::run\_command**\.

**To test the recipe**

1. Log in to the instance, if you have not done so already\.

1. From the command prompt, run the following commands, one at a time, to confirm that the SSH key was created:

   ```
   sudo cat /tmp/my-key
   
   sudo cat /tmp/my-key.pub
   ```

   The SSH private and public key's contents are displayed:

   ```
   -----BEGIN RSA PRIVATE KEY-----
   Proc-Type: 4,ENCRYPTED
   DEK-Info: AES-128-CBC,DEF7A09C...541583FA
   A5p9dCuo...wp0YYH1c
   -----END RSA PRIVATE KEY-----
   
   ssh-rsa AAAAB3N...KaNogZkT root@cookbooks-demo1
   ```

In the [next step](gettingstarted-cookbooks-run-script.md), you will update the cookbook to run a script on the instance\.