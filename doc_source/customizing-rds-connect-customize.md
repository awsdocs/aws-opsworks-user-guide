# Customize the Stack to Connect to the RDS Database<a name="customizing-rds-connect-customize"></a>

Once you have [created an RDS instance](customizing-rds-connect-create.md) to use as a back\-end database for the PHP application server, you can customize MyStack from [Getting Started with Chef 11 Linux Stacks](gettingstarted.md)\.

**To connect the PHP App Server to an RDS database**

1. Open the AWS OpsWorks Stacks console and create a stack with a PHP App Server layer that contains one instance and deploy SimplePHPApp, as described in [Getting Started with Chef 11 Linux Stacks](gettingstarted.md)\. This stack uses version1 of SimplePHPApp, which does not use a database connection\. 

1. [Update the stack configuration](workingstacks-edit.md) to use the custom cookbooks that include the `appsetup.rb` recipe, and related template and attribute files\.

   1. Set **Use custom Chef cookbooks** to **Yes**\.

   1. Set **Repository type** to **Git** and **Repository URL** to `git://github.com/amazonwebservices/opsworks-example-cookbooks.git`\.

1. Add the following to the stack's **Custom Chef JSON** box to assign the RDS connection data to the `[:database]` attributes that `appsetup.rb` uses to create the configuration file\.

   ```
   {
     "deploy": {
       "simplephpapp": {
         "database": {
           "username": "opsworksuser",
           "password": "your_password",
           "database": "rdsexampledb",
           "host": "rds_endpoint",
           "adapter": "mysql"
         }
       }
     }
   }
   ```

   Use the following attribute values:
   + **username**: The master user name that you specified when you created the RDS instance\.

     This example uses `opsworksuser`\.
   + **password**: The master password that you specified when you created the RDS instance\.

     Fill in the password that you specified\.
   + **database**: The database that you created when you created the RDS instance\.

     This example uses `rdsexampledb`\.
   + **host**: The RDS instance's endpoint, which you got from the RDS console when you created the instance in the previous section\. Don't include the port number\.
   + **adapter**: The adapter\.

     The RDS instance for this example uses MySQL, so **adapter** is set to `mysql`\. Unlike the other attributes, **adapter** is not used by `appsetup.rb`\. It is instead used by the PHP App Server layer's built\-in Configure recipe to create a different configuration file\.

1. [Edit the SimplePHPApp configuration](workingapps-editing.md) to specify a version of SimplePHPApp that uses a back\-end database, as follows:
   + **Document root**: Set this option to `web`\.
   + **Branch/Revision**: Set this option to `version2`\.

   Leave the remaining options unchanged\.

1. [Edit the PHP App Server layer](workinglayers-basics-edit.md) to set up the database connection by adding `phpapp::appsetup` to the layer's Deploy recipes\.

1. [Deploy the new SimplePHPApp version](workingapps-deploying.md)\.

1. When SimplePHPApp is deployed, run the application by going to the **Instances** page and clicking the php\-app1 instance's public IP address\. You should see the following page in your browser, which allows you to enter text and store it in the database\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gsb7.png)

**Note**  
If your stack has a MySQL layer, AWS OpsWorks Stacks automatically assigns the corresponding connection data to the `[:database]` attributes\. However, if you assign custom JSON to the stack that defines different `[:database]` values, they override the default values\. Because the `[:deploy]` attributes are installed on every instance, any recipes that depend on the `[:database]` attributes will use the custom connection data, not the MySQL layer's data for the\. If you want a particular application server layer to use the custom connection data, assign the custom JSON to the layer's Deploy event, and restrict that deployment to that layer\. For more information on how to use deployment attributes, see [Deploying Apps](workingapps-deploying.md)\. For more information on overriding AWS OpsWorks Stacks built\-in attributes, see [Overriding Attributes](workingcookbook-attributes.md)\.