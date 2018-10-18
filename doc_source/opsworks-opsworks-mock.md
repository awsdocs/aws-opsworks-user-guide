# Mocking the Stack Configuration and Deployment Attributes on Vagrant<a name="opsworks-opsworks-mock"></a>

**Note**  
This topic applies only to Linux instances\. Test Kitchen does not yet support Windows, so you will run all Windows examples on AWS OpsWorks Stacks instances\.

AWS OpsWorks Stacks adds [stack configuration and deployment attributes](workingcookbook-json.md) to the node object for each instance in your stack for every lifecycle event\. These attributes provide a snapshot of the stack configuration, including the configuration of each layer and its online instances, the configuration of each deployed app, and so on\. Because these attributes are in the node object, they can be accessed by any recipe; most recipes for AWS OpsWorks Stacks instances use one or more of these attributes\. 

An instance running in a Vagrant box is not managed by AWS OpsWorks Stacks, so its node object does not include any stack configuration and deployment attributes by default\. However, you can add a suitable set of attributes to the Test Kitchen environment\. Test Kitchen then adds the attributes to the instance's node object, and your recipes can access the attributes much like they would on an AWS OpsWorks Stacks instance\.

This topic shows how to obtain a copy of a suitable stack configuration and deployment attributes, install the attributes on an instance, and access them\.

**Note**  
If you are using Test Kitchen to run tests on your recipes, [fauxhai](https://github.com/customink/fauxhai) provides an alternative way to mock stack configuration and deployment JSON\.

**To set up the cookbook**

1. Create a subdirectory of `opsworks_cookbooks` named `printjson` and navigate to it\.

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md)\.

1. Add two subdirectories to `printjson`: `recipes` and `environments`\.

You could mock stack configuration and deployment attributes by adding an attribute file to your cookbook with the appropriate definitions, but a better approach is to use the Test Kitchen environment\. There are two basic approaches:
+ Add attribute definitions to `.kitchen.yml`\.

  This approach is most useful if you have just a few attributes\. For more information, see [kitchen\.yml](https://docs.chef.io/config_yml_kitchen.html)\.
+ Define the attributes in an environment file and reference the file in `.kitchen.yml`\.

  This approach is usually preferable for stack configuration and deployment attributes because the environment file is already in JSON format\. You can get a copy of the attributes in JSON format from a suitable AWS OpsWorks Stacks instance and just paste it in\. All of the examples use an environment file\.

The simplest way to create a stack configuration and deployment attributes for your cookbook is to create an appropriately configured stack and copy the resulting attributes from an instance as JSON\. To keep your Test Kitchen environment file manageable, you can then edit that JSON to have only the attributes that your recipes need\. The examples in this chapter are based on the stack from [Getting Started with Chef 11 Linux Stacks](gettingstarted.md), which is a simple PHP application server stack with a load balancer, PHP application servers, and a MySQL database server\.

**To create a stack configuration and deployment JSON**

1. Create MyStack as described in [Getting Started with Chef 11 Linux Stacks](gettingstarted.md), including deploying SimplePHPApp\. If you prefer, you can omit the second PHP App Server instance called for in [Step 4: Scale Out MyStack](gettingstarted-scale.md); the examples don't use those attributes\.

1. If you haven't already done so, start the `php-app1` instance, and then [log in with SSH](workinginstances-ssh.md)\.

1. In the terminal window, run the following [agent cli](agent.md) command:

   ```
   sudo opsworks-agent-cli get_json
   ```

   This command prints the instance's most recent stack configuration and deployment attributes to the terminal window in JSON format\.

1. Copy the JSON to a `.json` file and save it in a convenient location on your workstation\. The details depend on your SSH client\. For example, if you are using PuTTY on Windows, you can run the `Copy All to Clipboard` command, which copies all the text in the terminal window to the Windows clipboard\. You can then paste the contents into a `.json` file and edit the file to remove extraneous text\.

1. Edit MyStack JSON as needed\. Stack configuration and deployment attributes are numerous, and cookbooks typically use only a small subset of them\. To keep your environment file manageable, you can edit the JSON so that it retains the original structure but contains only the attributes that your cookbooks actually use\.

   This example uses a heavily edited version of the MyStack JSON that includes just two `['opsworks']['stack']` attributes, `['id]` and `['name']`\. Create an edited version of the MyStack JSON that looks something like the following:

   ```
   {
     "opsworks": {
       "stack": {
         "name": "MyStack",
         "id": "42dfd151-6766-4f1c-9940-ba79e5220b58",
       },
     },
   }
   ```

To get this JSON into the instance's node object, you need to add it to a Test Kitchen environment\.

**To add stack configuration and deployment attributes to the Test Kitchen environment**

1. Create an environment file named `test.json` with the following contents and save it to the cookbook's `environments` folder\.

   ```
   {
     "default_attributes": {
       "opsworks" : {
         "stack" : {
           "name" : "MyStack",
           "id" : "42dfd151-6766-4f1c-9940-ba79e5220b58"
         }
       }
     },
     "chef_type" : "environment",
     "json_class" : "Chef::Environment"
   }
   ```

   The environment file has the following elements:
   + `default_attributes` – The default attributes in JSON format\.

     These attributes are added to the node object with the `default` attribute type, which is the type used by all of the stack configuration and deployment JSON attributes\. This example uses the edited version of the stack configuration and deployment JSON shown earlier\.
   + `chef_type` – Set this element to `environment`\.
   + `json_class` – Set this element to `Chef::Environment`\.

1. Edit `.kitchen.yml` to define the Test Kitchen environment, as follows\.

   ```
   ---
   driver:
     name: vagrant
   
   provisioner:
     name: chef_solo
     environments_path: ./environments
   
   platforms:
     - name: ubuntu-12.04
   
   suites:
     - name: printjson 
       provisioner:
         solo_rb:
           environment: test
       run_list:
         - recipe[printjson::default]
       attributes:
   ```

   You define the environment by adding the following elements to the default `.kitchen.yml` created by `kitchen init`\.  
**provisioner**  
Add the following elements\.  
   + `name` – Set this element to `chef_solo`\.

     To replicate the AWS OpsWorks Stacks environment more closely, you could use [Chef client local mode](https://docs.chef.io/ctl_chef_client.html) instead of Chef solo\. Local mode is a Chef client option that uses a lightweight version of Chef server \(Chef Zero\) that runs locally on the instance instead of a remote server\. It enables your recipes to use Chef server features such as search or data bags without connecting to a remote server\.
   + `environments_path` – The cookbook subdirectory that contains the environment file, `./environments` for this example\.  
**suites:provisioner**  
Add a `solo_rb` element with an `environment` element set to the environment file's name, minus the \.json extension\. This example sets `environment` to `test`\.

1. Create a recipe file named `default.rb` with the following content and save it to the cookbook's `recipes` directory\.

   ```
   log "Stack name: #{node['opsworks']['stack']['name']}"
   log "Stack id: #{node['opsworks']['stack']['id']}"
   ```

   This recipe simply logs the two stack configuration and deployment values that you added to the environment\. Although the recipe is running locally in Virtual Box, you reference those attributes using the same node syntax that you would if the recipe were running on an AWS OpsWorks Stacks instance\.

1. Run `kitchen converge`\. You should see something like the following log output\.

   ```
   ...
   Converging 2 resources       
   Recipe: printjson::default       
     * log[Stack name: MyStack] action write[2014-07-01T23:14:09+00:00] INFO: Processing log[Stack name: MyStack] action write (printjson::default line 1)       
   [2014-07-01T23:14:09+00:00] INFO: Stack name: MyStack       
                
     * log[Stack id: 42dfd151-6766-4f1c-9940-ba79e5220b58] action write[2014-07-01T23:14:09+00:00] INFO: Processing log[Stack id: 42dfd151-6766-4f1c-9940-ba79e5220b58] action write (printjson::default line 2)       
   [2014-07-01T23:14:09+00:00] INFO: Stack id: 42dfd151-6766-4f1c-9940-ba79e5220b58       
   ...
   ```