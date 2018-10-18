# Step 14: Update the Cookbook to Use Iteration<a name="gettingstarted-cookbooks-iteration"></a>

Update your cookbook by adding a recipe that uses *iteration*, a technique that repeats recipe code multiple times\. This recipe displays messages in the log for a data bag item that contains multiple contents\. 

**To update the cookbook on the instance and to run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `iteration_demo.rb` that contains the following code:

   ```
   stack = search("aws_opsworks_stack").first
   Chef::Log.info("********** Content of 'custom_cookbooks_source' **********")
   
   stack["custom_cookbooks_source"].each do |content|
     Chef::Log.info("********** '#{content}' **********")
   end
   ```
**Note**  
Writing the preceding recipe code is shorter, more flexible, and less error\-prone than writing the following recipe code that does not use iteration:  

   ```
   stack = search("aws_opsworks_stack").first
   Chef::Log.info("********** Content of 'custom_cookbooks_source' **********")
   
   Chef::Log::info("********** '[\"type\", \"#{stack['custom_cookbooks_source']['type']}\"]' **********")
   Chef::Log::info("********** '[\"url\", \"#{stack['custom_cookbooks_source']['url']}\"]' **********")
   Chef::Log::info("********** '[\"username\", \"#{stack['custom_cookbooks_source']['username']}\"]' **********")
   Chef::Log::info("********** '[\"password\", \"#{stack['custom_cookbooks_source']['password']}\"]' **********")
   Chef::Log::info("********** '[\"ssh_key\", \"#{stack['custom_cookbooks_source']['ssh_key']}\"]' **********")
   Chef::Log::info("********** '[\"revision\", \"#{stack['custom_cookbooks_source']['revision']}\"]' **********")
   ```

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::iteration\_demo**\. 

**To test the recipe**

1. With the **Running command execute\_recipes** page displayed from the previous procedures, for **cookbooks\-demo1**, for **Log**, choose **show**\. The **execute\_recipes** log page is displayed\.

1. Scroll down through the log and find entries that look similar to the following:

   ```
   [2015-11-16T19:56:56+00:00] INFO: ********** Content of 'custom_cookbooks_source' **********
   [2015-11-16T19:56:56+00:00] INFO: ********** '["type", "s3"]' **********
   [2015-11-16T19:56:56+00:00] INFO: ********** '["url", "https://s3.amazonaws.com/opsworks-demo-bucket/opsworks_cookbook_demo.tar.gz"]' **********
   [2015-11-16T19:56:56+00:00] INFO: ********** '["username", "secret-key-value"]' **********
   [2015-11-16T19:56:56+00:00] INFO: ********** '["password", "secret-access-key-value"]' **********
   [2015-11-16T19:56:56+00:00] INFO: ********** '["ssh_key", nil]' **********
   [2015-11-16T19:56:56+00:00] INFO: ********** '["revision", nil]' **********
   ```

   This recipe displays messages in the log for a data bag item that contains multiple contents\. The data bag item is in the `aws_opsworks_stack` data bag\. The data bag item has content named `custom_cookbooks_source`\. Inside of this content are six contents named `type`, `url`, `username`, `password`, `ssh_key`, and `revision`; their values are also displayed\.

In the [next step](gettingstarted-cookbooks-conditional-logic.md), you will update the cookbook to run recipe code only if certain conditions are met\.