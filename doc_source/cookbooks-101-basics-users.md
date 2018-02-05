# Example 2: Managing Users<a name="cookbooks-101-basics-users"></a>

Another simple task is managing users on an instance\. The following recipe adds a new user to a Linux instance\.

```
user "myuser" do
  home "/home/newuser"
  shell "/bin/bash"
end
```

You use a [user](https://docs.chef.io/chef/resources.html#user) resource to manage users on both Linux and Windows systems, although some attributes apply to only one system\. The example creates a user named `myuser` and specifies their home directory and shell\. There is no action specified, so the resource uses the default `create` action\. You can add attributes to `user` to specify a variety of other settings, such as their password or group ID\. You can also use `user` for related user\-management tasks such as modifying user settings or deleting users\. For more information, see [user](https://docs.chef.io/chef/resources.html#user)\.

**To run the recipe**

1. Create a directory within `opsworks_cookbooks` named `newuser` and navigate to it\.

1. Create a `metadata.rb` file that contains the following code and save it to `newuser`\.

   ```
   name "newuser"
   version "0.1.0"
   ```

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md), and add a `recipes` directory inside the `newuser` directory\.

1.  Add `default.rb` file with the example recipe to the cookbook's `recipes` directory \. 

1. Run `kitchen converge` to execute the recipe\.

1. Use `kitchen login` to log in to the instance and verify the new user's existence by running `cat /etc/passwd`\. The `myuser` user should be at the bottom of the file\.