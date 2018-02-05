# Example 3: Creating Directories<a name="cookbooks-101-basics-directories"></a>

When you install a package on an instance, you often need to create some configuration files and place them in the appropriate directories\. However, those directories might not exist yet\. You might also need to create directories for data, log files, and so on\. For example, you first boot the Ubuntu 12\.04 LTS system that you use for most of the examples, the `/srv` directory has no subdirectories\. If you are installing an application server, you will probably want a `/srv/www/` directory and perhaps some subdirectories for data files, logs, and so on\. The following recipe creates `/srv/www/` on an instance\.

```
directory "/srv/www/" do
  mode 0755
  owner 'root'
  group 'root'
  action :create
end
```

You use a [`directory` resource](https://docs.chef.io/chef/resources.html#directory) to create and configure directories on both Linux and Windows systems, although some attributes are used differently\. The resource name is the default value for the resource's `path` attribute, so the example creates `/srv/www/` and specifies its `mode`, `owner`, and `group` properties\.

**To run the recipe**

1. Create a directory inside `opsworks_cookbooks` named `createdir` and navigate to it\.

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md), and add a `recipes` directory within `createdir`\.

1.  Add a `default.rb` file with the recipe code to the cookbook's `recipes` subdirectory\. 

1. Run `kitchen converge` to execute the recipe\.

1. Run `kitchen login`, navigate to `/srv` and verify that it has a `www` subdirectory\.

1. Run `exit` to return to your workstation but leave the instance running\.

**Note**  
To create a directory relative to your home directory on the instance, use `#{ENV['HOME']}` to represent the home directory\. For example, the following creates the `~/shared` directory\.  

```
directory "#{ENV['HOME']}/shared" do
  ...
end
```

Suppose that you want to create a more deeply nested directory, such as `/srv/www/shared`\. You could modify the preceding recipe as follows\.

```
directory "/srv/www/shared" do
  mode 0755
  owner 'root'
  group 'root'
  action :create
end
```

**To run the recipe**

1.  Replace the code in `default.rb` with the preceding recipe\. 

1. Run `kitchen converge` from the `createdir` directory\.

1. To verify that the directory was indeed created, run `kitchen login`, navigate to `/srv/www`, and verify that it contains a `shared` subdirectory\. 

1. Run `kitchen destroy` to shut the instance down\.

You will notice the `kitchen converge` command ran much faster\. That's because the instance is already running, so there's no need to boot the instance, install Chef, and so on\. Test Kitchen just to copies the updated cookbook to the instance and starts a Chef run\.

Now run `kitchen converge` again, which executes the recipe on a fresh instance\. You'll now see the following result\.

```
Chef Client failed. 0 resources updated in 1.908125788 seconds       
[2014-06-20T20:54:26+00:00] ERROR: directory[/srv/www/shared] (createdir::default line 1) had an error: Chef::Exceptions::EnclosingDirectoryDoesNotExist: Parent directory /srv/www does not exist, cannot create /srv/www/shared       
[2014-06-20T20:54:26+00:00] FATAL: Chef::Exceptions::ChildConvergeError: Chef run process exited unsuccessfully (exit code 1)       
>>>>>> Converge failed on instance <default-ubuntu-1204>.
>>>>>> Please see .kitchen/logs/default-ubuntu-1204.log for more details
>>>>>> ------Exception-------
>>>>>> Class: Kitchen::ActionFailed
>>>>>> Message: SSH exited (1) for command: [sudo -E chef-solo --config /tmp/kitchen/solo.rb --json-attributes /tmp/kitchen/dna.json  --log_level info]
>>>>>> ----------------------
```

What happened? The problem is that by default, a `directory` resource can create only one directory at a time; it can't create a chain of directories\. The reason the recipe worked earlier is that the very first recipe you ran on the instance had already created `/srv/www`, so creating `/srv/www/shared` created only one subdirectory\.

**Note**  
When you run `kitchen converge`, make sure you know whether you are running your recipes on a new or existing instance\. You might get different results\.

To create a chain of subdirectories, add a `recursive` attribute to `directory` and set it to `true`\. The following recipe creates `/srv/www/shared` directly on a clean instance\.

```
directory "/srv/www/shared" do
  mode 0755
  owner 'root'
  group 'root'
  recursive true
  action :create
end
```