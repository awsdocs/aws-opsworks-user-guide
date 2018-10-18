# Step 3: Create and Deploy a Custom Cookbook<a name="using-s3-cookbook"></a>

The stack is not quite ready yet:
+ Your application needs some information to access to the MySQL database server and the Amazon S3 bucket, such as the database host name and the Amazon S3 bucket name\.
+ You need to set up a database in the MySQL database server and create a table to hold the photos' metadata\.

You could handle these tasks manually, but a better approach is to implement Chef *recipe* and have AWS OpsWorks Stacks run the recipe automatically on the appropriate instances\. Chef recipes are specialized Ruby applications that AWS OpsWorks Stacks uses to perform tasks on instances such as installing packages or creating configuration files\. They are packaged in a *cookbook*, which can contain multiple recipes and related files such as templates for configuration files\. The cookbook is placed in a repository such as GitHub, and must have a standard directory structure\. If you don't yet have a custom cookbook repository, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md) for information on how to set one up\.

For this example, the cookbook has been implemented for you and is stored in a [public GitHub repository](https://github.com/amazonwebservices/opsworks-example-cookbooks/tree/master/photoapp)\. The cookbook contains two recipes, `appsetup.rb` and `dbsetup.rb`, and a template file, `db-connect.php.erb`\.

The `appsetup.rb` recipe creates a configuration file that contains the information that the application needs to access the database and the Amazon S3 bucket\. It is basically a lightly modified version of the `appsetup.rb` recipe described in [Connect the Application to the Database](gettingstarted-db-recipes.md#gettingstarted-db-recipes-appsetup)\. The primary difference is the variables that are passed to the template, which represent the access information\.

```
variables(
  :host =>     (deploy[:database][:host] rescue nil),
  :user =>     (deploy[:database][:username] rescue nil),
  :password => (deploy[:database][:password] rescue nil),
  :db =>       (deploy[:database][:database] rescue nil),
  :table =>    (node[:photoapp][:dbtable] rescue nil),
  :s3bucket => (node[:photobucket] rescue nil)
)
```

The first four attributes define database connection settings, and are automatically defined by AWS OpsWorks Stacks when you create the MySQL instance\.

There are two differences between these variables and the ones in the original recipe:
+ Like the original recipe, the `table` variable represents the name of the database table that is created by `dbsetup.rb`, and is set to the value of an attribute that is defined in the cookbook's attributes file\.

  However, the attribute has a different name: `[:photoapp][:dbtable]`\.
+ The `s3bucket` variable is specific to this example and is set to the value of an attribute that represents the Amazon S3 bucket name, `[:photobucket]`\.

   `[:photobucket]` is defined by using custom JSON, as described later\. For more information on attributes, see [Attributes](workingcookbook-installingcustom-components-attributes.md)

For more information on attributes, see [Attributes](workingcookbook-installingcustom-components-attributes.md)\.

The `dbsetup.rb` recipe sets up a database table to hold each photo's metadata\. It basically is a lightly modified version of the `dbsetup.rb` recipe described in [Set Up the Database](gettingstarted-db-recipes.md#gettingstarted-db-recipes-dbsetup); see that topic for a detailed description\. 

```
node[:deploy].each do |app_name, deploy|
  execute "mysql-create-table" do
    command "/usr/bin/mysql -u#{deploy[:database][:username]} -p#{deploy[:database][:password]} #{deploy[:database][:database]} -e'CREATE TABLE #{node[:photoapp][:dbtable]}(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    url VARCHAR(255) NOT NULL,
    caption VARCHAR(255),
    PRIMARY KEY (id)
  )'"
    not_if "/usr/bin/mysql -u#{deploy[:database][:username]} -p#{deploy[:database][:password]} #{deploy[:database][:database]} -e'SHOW TABLES' | grep #{node[:photoapp][:dbtable]}"
    action :run
  end
end
```

The only difference between this example and the original recipe is the database schema, which has three columns that contain the ID, URL, and caption of each photo that is stored on the Amazon S3 bucket\.

The recipes are already implemented, so all you need to do is deploy the photoapp cookbook to each instance's cookbook cache\. AWS OpsWorks Stacks then runs the cached recipes when the appropriate lifecycle event occurs, as described later\.

**To deploy the photoapp cookbook**

1. On the AWS OpsWorks Stacks **Stack** page, choose **Stack Settings** and then choose **Edit**\.

1. In the **Configuration Management** section:
   + Set **Use custom Chef cookbooks** to **Yes**\.
   + Set **Repository type** to Git\.
   + Set **Repository URL** to **git://github\.com/amazonwebservices/opsworks\-example\-cookbooks\.git**\.

1. On the **Stack** page, choose **Run Command**, select the **Update Custom Cookbooks** stack command, and then choose **Update Custom Cookbooks** to install the new cookbook in instance cookbook caches\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/redis_walkthrough_command.png)