# Obtaining Attribute Values Directly<a name="cookbooks-101-opsworks-opsworks-stack-config-node"></a>

**Note**  
This approach works only for Linux stacks\.

[Mocking the Stack Configuration and Deployment Attributes on Vagrant](opsworks-opsworks-mock.md) shows how to obtain stack configuration and deployment data by using node syntax to directly reference particular attributes\. This is sometimes the best approach\. However, many attributes are defined in collections or lists whose contents and names can vary from stack to stack and over time for a particular stack\. For example, the `deploy` attribute contains a list of app attributes, which are named with the app's short name\. This list, including the app attribute names, typically varies from stack to stack and even from deployment to deployment\. 

It is often more useful, and sometimes even necessary, to obtain the required data by enumerating the attributes in a list or collection\. For example, suppose that you want to know the public IP addresses of your stack's instances\. That information is in the `['opsworks']['layers']` attribute, which is set to a hash table that contains one element for each of the stack's layers, named with the layer's shortname\. Each layer element is set to a hash table containing the layer's attributes, one of which is `['instances']`\. That element in turn is set to yet another hash table containing an attribute for each of the layer's instances, named with the instance's shortname\. Each instance attribute is set to still another hash table that contains the instance attributes, including `['ip']`, which represents the public IP address\. If you are having trouble visualizing this, the following procedure includes an example in JSON format\.

This example shows how to obtain data from the stack configuration and deployment JSON for a stack's layers\.

**To set up the cookbook**

1. Create a directory within `opsworks_cookbooks` named `listip` and navigate to it\.

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md)\.

1. Add two directories to `listip`: `recipes` and `environments`\.

1. Create an edited JSON version of the MyStack configuration and deployment attributes that contains the relevant attributes\. It should look something like the following\.

   ```
   {
     "opsworks": {
       "layers": {
         "php-app": {
           "name": "PHP App Server",
           "id": "efd36017-ec42-4423-b655-53e4d3710652",
           "instances": {
             "php-app1": {
               "ip": "192.0.2.0"
             }
           }
         },
         "db-master": {
           "name": "MySQL",
           "id": "2d8e0b9a-0d29-43b7-8476-a9b2591a7251",
           "instances": {
             "db-master1": {
               "ip": "192.0.2.5"
             }
           }
         },
         "lb": {
           "name": "HAProxy",
           "id": "d5c4dda9-2888-4b22-b1ea-6d44c7841193",
           "instances": {
             "lb1": {
               "ip": "192.0.2.10"
             }
           }
         }
       }
     }
   }
   ```

1. Create an environment file named `test.json`, paste the example JSON into `default_attributes`, and save the file to the cookbook's `environments` folder\. The file should look something the following \(for brevity, most of the example JSON is represented by an ellipsis\)\.

   ```
   {
     "default_attributes" : {
       "opsworks": {
         "layers": {
           ...
         }
       }
     },
     "chef_type" : "environment",
     "json_class" : "Chef::Environment"
   }
   ```

1. Replace the text in `.kitchen.yml` with the following\.

   ```
   ---
   driver:
     name: vagrant
   
   provisioner:
     name: chef_zero
     environments_path: ./environment
   
   platforms:
     - name: ubuntu-12.04
   
   suites:
     - name: listip 
       provisioner:
         client_rb:
           environment: test
       run_list:
         - recipe[listip::default]
       attributes:
   ```

After the cookbook is set up, you can use the following recipe to log the layer IDs\.

```
node['opsworks']['layers'].each do |layer, layerdata|
  log "#{layerdata['name']} : #{layerdata['id']}"
end
```

The recipe enumerates the layers in `['opsworks']['layers']` and logs each layer's name and ID\.

**To run the layer ID logging recipe**

1. Create a file named `default.rb` with the example recipe and save it to the `recipes` directory\.

1. Run `kitchen converge`\.

The relevant part of the output should look something like the following\.

```
Recipe: listip::default       
  * log[PHP App Server : efd36017-ec42-4423-b655-53e4d3710652] action write[2014-07-17T22:56:19+00:00] INFO: Processing log[PHP App Server : efd36017-ec42-4423-b655-53e4d3710652] action write (listip::default line 4)       
[2014-07-17T22:56:19+00:00] INFO: PHP App Server : efd36017-ec42-4423-b655-53e4d3710652       
       
       
  * log[MySQL : 2d8e0b9a-0d29-43b7-8476-a9b2591a7251] action write[2014-07-17T22:56:19+00:00] INFO: Processing log[MySQL : 2d8e0b9a-0d29-43b7-8476-a9b2591a7251] action write (listip::default line 4)       
[2014-07-17T22:56:19+00:00] INFO: MySQL : 2d8e0b9a-0d29-43b7-8476-a9b2591a7251       
       
       
  * log[HAProxy : d5c4dda9-2888-4b22-b1ea-6d44c7841193] action write[2014-07-17T22:56:19+00:00] INFO: Processing log[HAProxy : d5c4dda9-2888-4b22-b1ea-6d44c7841193] action write (listip::default line 4)       
[2014-07-17T22:56:19+00:00] INFO: HAProxy : d5c4dda9-2888-4b22-b1ea-6d44c7841193
```

To list the instances' IP addresses, you will need a nested loop like the following\.

```
node['opsworks']['layers'].each do |layer, layerdata|
  log "#{layerdata['name']} : #{layerdata['id']}"
  layerdata['instances'].each do |instance, instancedata|
    log "Public IP: #{instancedata['ip']}"
  end
end
```

The inner loop iterates over each layer's instances and logs the IP addresses\.

**To run the instance IP logging recipe**

1. Replace the code in `default.rb` with the example recipe\.

1. Run `kitchen converge` to execute the recipe\.

The relevant part of the output should look something like the following\.

```
  * log[PHP App Server : efd36017-ec42-4423-b655-53e4d3710652] action write[2014-07-17T23:09:34+00:00] INFO: Processing log[PHP App Server : efd36017-ec42-4423-b655-53e4d3710652] action write (listip::default line 2)       
[2014-07-17T23:09:34+00:00] INFO: PHP App Server : efd36017-ec42-4423-b655-53e4d3710652       
       
       
  * log[Public IP: 192.0.2.0] action write[2014-07-17T23:09:34+00:00] INFO: Processing log[Public IP: 192.0.2.0] action write (listip::default line 4)       
[2014-07-17T23:09:34+00:00] INFO: Public IP: 192.0.2.0       
       
       
  * log[MySQL : 2d8e0b9a-0d29-43b7-8476-a9b2591a7251] action write[2014-07-17T23:09:34+00:00] INFO: Processing log[MySQL : 2d8e0b9a-0d29-43b7-8476-a9b2591a7251] action write (listip::default line 2)       
[2014-07-17T23:09:34+00:00] INFO: MySQL : 2d8e0b9a-0d29-43b7-8476-a9b2591a7251       
       
       
  * log[Public IP: 192.0.2.5] action write[2014-07-17T23:09:34+00:00] INFO: Processing log[Public IP: 192.0.2.5] action write (listip::default line 4)       
[2014-07-17T23:09:34+00:00] INFO: Public IP: 192.0.2.5       
       
       
  * log[HAProxy : d5c4dda9-2888-4b22-b1ea-6d44c7841193] action write[2014-07-17T23:09:34+00:00] INFO: Processing log[HAProxy : d5c4dda9-2888-4b22-b1ea-6d44c7841193] action write (listip::default line 2)       
[2014-07-17T23:09:34+00:00] INFO: HAProxy : d5c4dda9-2888-4b22-b1ea-6d44c7841193       
       
       
  * log[Public IP: 192.0.2.10] action write[2014-07-17T23:09:34+00:00] INFO: Processing log[Public IP: 192.0.2.10] action write (listip::default line 4)       
[2014-07-17T23:09:34+00:00] INFO: Public IP: 192.0.2.10
```

When you are finished, run `kitchen destroy`; the next topic uses a new cookbook\.

**Note**  
One of the most common reasons for enumerating a stack configuration and deployment JSON collection is to obtain data for a particular deployed app, such as its deployment directory\. For an example, see [Deploy Recipes](create-custom-deploy.md)\.