# How to Set up a Database Connection<a name="customizing-rds-setup"></a>

You set up the connection between an application server and its back\-end database by using a custom recipe\. The recipe configures the application server as required, typically by creating a configuration file\. The recipe gets the connection data such as the host and database name from a set of attributes in the [stack configuration and deployment attributes](workingcookbook-json.md) that AWS OpsWorks Stacks installs on every instance\.

For example, Step 2 of [Getting Started with Chef 11 Linux Stacks](gettingstarted.md) is based on a stack named MyStack with two layers, PHP App Server and MySQL, each with one instance\. You deploy an app named SimplePHPApp to the PHP App Server instance that uses the database on the MySQL instance as a back\-end data store\. When you deploy the application, AWS OpsWorks Stacks installs stack configuration and deployment attributes that contain the database connection information\. The following example shows the database connection attributes, represented as JSON:

```
{
  ...
  "deploy": {
    "simplephpapp": {
      ...
      "database": {
        "reconnect": true,
        "password": null,
        "username": "root",
        "host": null,
        "database": "simplephpapp"
        ...
      },
      ...
    }
  }
}
```

The attribute values are supplied by AWS OpsWorks Stacks, and are either generated or based on user provided information\.

To allow SimplePHPApp to access the data store, you must set up the connection between the PHP application server and the MySQL database by assigning a custom recipe named `appsetup.rb` to the PHP App Server layer's Deploy [lifecycle event](workingcookbook-events.md)\. When you deploy SimplePHPApp, AWS OpsWorks Stacks runs `appsetup.rb`, which creates a configuration file named `db-connect.php` that sets up the connection, as shown in the following excerpt\.

```
node[:deploy].each do |app_name, deploy|
  ...
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
    ...
  end
end
```

The variables that characterize the connection—`host`, `user`, and so on—are set the corresponding values from the [deploy JSON's](workingcookbook-json.md#workingcookbook-json-deploy) `[:deploy][:app_name][:database]` attributes\. For simplicity, the example assumes that you have already created a table named `urler`, so the table name is represented by `[:phpapp][:dbtable]` in the cookbook's attributes file\.

This recipe can actually connect the PHP application server to any MySQL database server, not just members of a MySQL layer\. To use a different MySQL server, you just have to set the `[:database]` attributes to values that are appropriate for your server, which you can do by using [custom JSON](workingstacks-json.md)\. AWS OpsWorks Stacks then incorporates those attributes and values into the stack configuration and deployment attributes and `appsetup.rb` uses them to create the template that sets up the connection\. For more information on overriding stack configuration and deployment JSON, see [Overriding Attributes](workingcookbook-attributes.md)\.