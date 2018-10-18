# A Short Digression: Cookbooks, Recipes, and AWS OpsWorks Stacks Attributes<a name="gettingstarted-db-recipes"></a>

You now have app and database servers, but they aren't quite ready to use\. You still need to set up the database and configure the app's connection settings\. AWS OpsWorks Stacks doesn't handle these tasks automatically, but it does support Chef cookbooks, recipes, and dynamic attributes\. You can implement a pair of recipes, one to set up the database and one to configure the app's connection settings, and have AWS OpsWorks Stacks run them for you\.

The phpapp cookbook, which contains the required recipes, is already implemented and ready for use; you can just skip to [Step 3\.3: Add the Custom Cookbooks to MyStack](gettingstarted-db-cookbooks.md) if you prefer\. If you'd like to know more, this section provides some background on cookbooks and recipes and describes how the recipes work\. To see the cookbook itself, go to the [phpapp cookbook](https://github.com/amazonwebservices/opsworks-example-cookbooks/tree/master/phpapp)\.

**Topics**
+ [Recipes and Attributes](#gettingstarted-db-recipes-attributes)
+ [Set Up the Database](#gettingstarted-db-recipes-dbsetup)
+ [Connect the Application to the Database](#gettingstarted-db-recipes-appsetup)

## Recipes and Attributes<a name="gettingstarted-db-recipes-attributes"></a>

A Chef recipe is basically a specialized Ruby application that performs tasks on an instance such as installing packages, creating configuration files, executing shell commands, and so on\. Groups of related recipes are organized into *cookbooks*, which also contain supporting files such as templates for creating configuration files\.

AWS OpsWorks Stacks has a set of cookbooks that support the built\-in layers\. You can also create custom cookbooks with your own recipes to perform custom tasks on your instances\. This topic provides a brief introduction to recipes and shows how to use them to set up the database and configure the app's connection settings\. For more information on cookbooks and recipes, see [Cookbooks and Recipes](workingcookbook.md) or [Customizing AWS OpsWorks Stacks](customizing.md)\.

Recipes usually depend on Chef *attributes* for input data: 
+ Some of these attributes are defined by Chef and provide basic information about the instance such as the operating system\. 
+ AWS OpsWorks Stacks defines a set of attributes that contain information about the stack—such as the layer configurations—and about deployed apps—such as the app repository\.

  You can add custom attributes to this set by assigning [custom JSON](workingstacks-json.md) to the stack or deployment\.
+ Your cookbooks can also define attributes, which are specific to the cookbook\. 

  The phpapp cookbook attributes are defined in `attributes/default.rb`\.

For a complete list of AWS OpsWorks Stacks attributes, see [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md) and [Built\-in Cookbook Attributes](attributes-recipes.md)\. For more information, see [Overriding Attributes](workingcookbook-attributes.md)\.

Attributes are organized in a hierarchical structure, which can be represented as a JSON object\. The following example shows a JSON representation of the deployment attributes for SimplePHPApp, which includes the values that you need for the task at hand\.

```
"deploy": {
  "simplephpapp": {
    "sleep_before_restart": 0,
    "rails_env": null,
    "document_root": "web",
    "deploy_to": "/srv/www/simplephpapp",
    "ssl_certificate_key": null,
    "ssl_certificate": null,
    "deploying_user": null,
    "ssl_certificate_ca": null,
    "ssl_support": false,
    "migrate": false,
    "mounted_at": null,
    "application": "simplephpapp",
    "auto_bundle_on_deploy": true,
    "database": {
        "reconnect": true,
        "database": "simplephpapp",
        "host": null,
        "adapter": "mysql",
        "data_source_provider": "stack",
        "password": "d1zethv0sm",
        "port": 3306,
        "username": "root"
    },
    "scm": {
      "revision": "version2",
      "scm_type": "git",
      "user": null,
      "ssh_key": null,
      "password": null,
      "repository": "git://github.com/amazonwebservices/opsworks-demo-php-simple-app.git"
    },
    "symlink_before_migrate": {
      "config/opsworks.php": "opsworks.php"
    },
    "application_type": "php",
    "domains": [
      "simplephpapp"
    ],
    "memcached": {
      "port": 11211,
      "host": null
    },
    "symlinks": {
    },
    "restart_command": "echo 'restarting app'"
  }
```

You incorporate this data into your application by using Chef node syntax, like the following:

```
[:deploy][:simplephpapp][:database][:username]
```

The `deploy` node has a single app node, `simplephpapp`, that contains information about the app's database, Git repository, and so on\. The example represents the value of the database user name, which resolves to `root`\.

## Set Up the Database<a name="gettingstarted-db-recipes-dbsetup"></a>

The MySQL layer's built\-in Setup recipes automatically create a database for the app named with the app's shortname, so for this example you already have a database named simplephpapp\. However, you need to finish the setup by creating a table for the app to store its data\. You could create the table manually, but a better approach is to implement a custom recipe to handle the task, and have AWS OpsWorks Stacks run it for you\. This section describes how the recipe, `dbsetup.rb`, is implemented\. The procedure for having AWS OpsWorks Stacks run the recipe is described later\.

To see the recipe in the repository, go to [dbsetup\.rb](https://github.com/amazonwebservices/opsworks-example-cookbooks/blob/master/phpapp/recipes/dbsetup.rb)\. The following example shows the `dbsetup.rb` code\. 

```
node[:deploy].each do |app_name, deploy|
  execute "mysql-create-table" do
    command "/usr/bin/mysql -u#{deploy[:database][:username]} -p#{deploy[:database][:password]} #{deploy[:database][:database]} -e'CREATE TABLE #{node[:phpapp][:dbtable]}(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    author VARCHAR(63) NOT NULL,
    message TEXT,
    PRIMARY KEY (id)
  )'"
    not_if "/usr/bin/mysql -u#{deploy[:database][:username]} -p#{deploy[:database][:password]} #{deploy[:database][:database]} -e'SHOW TABLES' | grep #{node[:phpapp][:dbtable]}"
    action :run
  end
end
```

This recipe iterates over the apps in the `deploy` node\. This example has only one app, but it's possible for a `deploy` node to have multiple apps\.

`execute` is a *Chef resource* that executes a specified command\. In this case, it's a MySQL command that creates a table\. The `not_if` directive ensures that the command does not run if the specified table already exists\. For more information on Chef resources, see [About Resources and Providers](https://docs.chef.io/resource.html)\.

The recipe inserts attribute values into the command string, using the node syntax discussed earlier\. For example, the following inserts the database's user name\.

```
#{deploy[:database][:username]}
```

Let's unpack this somewhat cryptic code:
+ For each iteration, `deploy` is set to the current app node, so it resolves to `[:deploy][:app_name]`\. For this example, it resolves to `[:deploy][:simplephpapp]`\.
+ Using the deployment attribute values shown earlier, the entire node resolves to `root`\.
+ You wrap the node in \#\{ \} to insert it into a string\.

Most of the other nodes resolve in a similar way\. The exception is `#{node[:phpapp][:dbtable]}`, which is defined by the custom cookbook's attributes file and resolves to the table name, `urler`\. The actual command that runs on the MySQL instance is therefore:

```
"/usr/bin/mysql 
    -uroot
    -pvjud1hw5v8
    simplephpapp
    -e'CREATE TABLE urler(
       id INT UNSIGNED NOT NULL AUTO_INCREMENT,
       author VARCHAR(63) NOT NULL,
       message TEXT,
       PRIMARY KEY (id))'
"
```

This command creates a table named `urler` with id, author, and message fields, using the credentials and database name from the deployment attributes\.

## Connect the Application to the Database<a name="gettingstarted-db-recipes-appsetup"></a>

The second piece of the puzzle is the application, which needs connection information such as the database password to access the table\. SimplePHPApp effectively has only one working file, `app.php`; all `index.php` does is load `app.php`\. 

`app.php` includes `db-connect.php`, which handles the database connection, but that file is not in the repository\. You can't create `db-connect.php` in advance because it defines the database based on the particular instance\. Instead, the `appsetup.rb` recipe generates `db-connect.php` using connection data from the deployment attributes\.

To see the recipe in the repository, go to [appsetup\.rb](https://github.com/amazonwebservices/opsworks-example-cookbooks/blob/master/phpapp/recipes/appsetup.rb)\. The following example shows the `appsetup.rb` code\. 

```
node[:deploy].each do |app_name, deploy|
  script "install_composer" do
    interpreter "bash"
    user "root"
    cwd "#{deploy[:deploy_to]}/current"
    code <<-EOH
    curl -s https://getcomposer.org/installer | php
    php composer.phar install
    EOH
  end

  template "#{deploy[:deploy_to]}/current/db-connect.php" do
    source "db-connect.php.erb"
    mode 0660
    group deploy[:group]

    if platform?("ubuntu")
      owner "www-data"
    elsif platform?("amazon")   
      owner "apache"
    end

    variables(
      :host =>     (deploy[:database][:host] rescue nil),
      :user =>     (deploy[:database][:username] rescue nil),
      :password => (deploy[:database][:password] rescue nil),
      :db =>       (deploy[:database][:database] rescue nil),
      :table =>    (node[:phpapp][:dbtable] rescue nil)
    )

   only_if do
     File.directory?("#{deploy[:deploy_to]}/current")
   end
  end
end
```

Like `dbsetup.rb`, `appsetup.rb` iterates over apps in the `deploy` node—just simplephpapp again—\. It runs a code block with a `script` resource and a `template` resource\.

The `script` resource installs [Composer](http://www.getcomposer.org)—a dependency manager for PHP applications\. It then runs Composer's `install` command to install the dependencies for the sample application to the app's root directory\.

The `template` resource generates `db-connect.php` and puts it in `/srv/www/simplephpapp/current`\. Note the following:
+ The recipe uses a conditional statement to specify the file owner, which depends on the instance's operating system\.
+ The `only_if` directive tells Chef to generate the template only if the specified directory exists\.

A `template` resource operates on a template that has essentially the same content and structure as the associated file but includes placeholders for various data values\. The `source` parameter specifies the template, `db-connect.php.erb`, which is in the phpapp cookbook's `templates/default` directory, and contains the following:

```
<?php 
  define('DB_NAME', '<%= @db%>');
  define('DB_USER', '<%= @user%>');
  define('DB_PASSWORD', '<%= @password%>');
  define('DB_HOST', '<%= @host%>');
  define('DB_TABLE', '<%= @table%>');
?>
```

When Chef processes the template, it replaces the `<%= =>` placeholders with the value of the corresponding variables in the template resource, which are in turn drawn from the deployment attributes\. The generated file is therefore:

```
<?php 
  define('DB_NAME', 'simplephpapp');
  define('DB_USER', 'root');
  define('DB_PASSWORD', 'ujn12pw');
  define('DB_HOST', '10.214.175.110');
  define('DB_TABLE', 'urler');
?>
```