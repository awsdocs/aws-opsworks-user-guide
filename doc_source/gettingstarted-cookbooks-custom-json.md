# Step 12: Update the Cookbook to Use Custom JSON<a name="gettingstarted-cookbooks-custom-json"></a>

Update your cookbook by adding a recipe that references custom JSON that is stored on the instance\.

You can specify information in custom JSON format whenever you create, update, or clone a stack or when you run a deployment or stack command\. This is useful, for example, for making a small, unchanging portion of data available to your recipes on the instance instead of getting this data from a database\. For more information, see [Using Custom JSON](workingstacks-json.md)\. 

For this walkthrough, you will use custom JSON to provide some fictitious information about a customer invoice\. The custom JSON is described later in this step\.

**To update the cookbook on the instance and run the new recipe**

1. On your local workstation, in the `recipes` subdirectory in the `opsworks_cookbook_demo` directory, create a file named `custom_json.rb` that contains the following recipe code: 

   ```
   Chef::Log.info("********** For customer '#{node['customer-id']}' invoice '#{node['invoice-number']}' **********")
   Chef::Log.info("********** Invoice line number 1 is a '#{node['line-items']['line-1']}' **********")
   Chef::Log.info("********** Invoice line number 2 is a '#{node['line-items']['line-2']}' **********")
   Chef::Log.info("********** Invoice line number 3 is a '#{node['line-items']['line-3']}' **********")
   ```

   This recipe displays messages in the log about values in the custom JSON\.

1. At the terminal or command prompt, use the tar command create a new version of the `opsworks_cookbook_demo.tar.gz` file, which contains the `opsworks_cookbook_demo` directory and its updated contents\.

1. Upload the updated `opsworks_cookbook_demo.tar.gz` file to your S3 bucket\.

1. Follow the procedures in [Step 5: Update the Cookbook on the Instance and Run the Recipe](gettingstarted-cookbooks-copy-cookbook.md) to update the cookbook on the instance and to run the recipe\. In the "To run the recipe" procedure, for **Recipes to execute**, type **opsworks\_cookbook\_demo::custom\_json**\. For **Advanced**, **Custom Chef JSON**, type the following custom JSON:

   ```
   {
     "customer-id": "0123",
     "invoice-number": "9876",
     "line-items": {
       "line-1": "tractor",
       "line-2": "passenger car",
       "line-3": "trailer"
     }
   }
   ```

**To test the recipe**

1. With the **Running command execute\_recipes** page displayed from the previous procedures, for **cookbooks\-demo1**, for **Log**, choose **show**\. The **execute\_recipes** log page is displayed\.

1. Scroll down through the log to find entries that look similar to the following:

   ```
   [2015-11-14T14:18:30+00:00] INFO: ********** For customer '0123' invoice '9876' **********
   [2015-11-14T14:18:30+00:00] INFO: ********** Invoice line number 1 is a 'tractor' **********
   [2015-11-14T14:18:30+00:00] INFO: ********** Invoice line number 2 is a 'passenger car' **********
   [2015-11-14T14:18:30+00:00] INFO: ********** Invoice line number 3 is a 'trailer' **********
   ```

   These entries display information from the custom JSON that was typed in the **Advanced**, **Custom Chef JSON** box\.

In the [next step](gettingstarted-cookbooks-data-bags.md), you will update the cookbook to get information from data bags, which are collections of stack settings that AWS OpsWorks Stacks stores on each instance\.