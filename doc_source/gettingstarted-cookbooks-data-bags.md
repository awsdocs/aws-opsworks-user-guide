# Step 13: Update the Cookbook to Use Data Bags<a name="gettingstarted-cookbooks-data-bags"></a>

Update your cookbook by adding a recipe that references stack settings that AWS OpsWorks Stacks stores on the instance in a set of data bags\. This recipe displays messages in the log about specific stack settings that are stored on the instance\. For more information, see the [AWS OpsWorks Stacks Data Bag Reference](data-bags.md)\.

**To update the cookbook on the instance and to run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `data_bags.rb` that contains the following code: 

   ```
   instance = search("aws_opsworks_instance").first
   layer = search("aws_opsworks_layer").first
   stack = search("aws_opsworks_stack").first
   
   Chef::Log.info("********** This instance's instance ID is '#{instance['instance_id']}' **********")
   Chef::Log.info("********** This instance's public IP address is '#{instance['public_ip']}' **********")
   Chef::Log.info("********** This instance belongs to the layer '#{layer['name']}' **********")
   Chef::Log.info("********** This instance belongs to the stack '#{stack['name']}' **********")
   Chef::Log.info("********** This stack gets its cookbooks from '#{stack['custom_cookbooks_source']['url']}' **********")
   ```

   This recipe displays messages in the log about specific stack settings that are stored on the instance\.

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::data\_bags**\. 

**To test the recipe**

1. With the **Running command execute\_recipes** page displayed from the previous procedure, for **cookbooks\-demo1**, for **Log**, choose **show**\. The **execute\_recipes** log page is displayed\.

1. Scroll down through the log and find entries that look similar to the following:

   ```
   [2015-11-14T14:39:06+00:00] INFO: ********** This instance's instance ID is 'f80fa119-81ab-4c3c-883d-6028e52c89EX' **********
   [2015-11-14T14:39:06+00:00] INFO: ********** This instance's public IP address is '192.0.2.0' **********
   [2015-11-14T14:39:06+00:00] INFO: ********** This instance belongs to the layer 'MyCookbooksDemoLayer' **********
   [2015-11-14T14:39:06+00:00] INFO: ********** This instance belongs to the stack 'MyCookbooksDemoStack' **********
   [2015-11-14T14:39:06+00:00] INFO: ********** This stack gets its cookbooks from 'https://s3.amazonaws.com/opsworks-demo-bucket/opsworks_cookbook_demo.tar.gz' **********
   ```

   This recipe displays messages about specific stack settings that are stored on the instance\.

In the [next step](gettingstarted-cookbooks-iteration.md), you will update the cookbook to run recipe code multiple times\.