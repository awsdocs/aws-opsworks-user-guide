# Example 7: Running Commands and Scripts<a name="cookbooks-101-basics-commands"></a>

Chef resources can handle a wide variety of tasks on an instance, but it is sometimes preferable to use a shell command or a script\. For example, you might already have scripts that you use to accomplish certain tasks, and it will be easier to continue using them rather than implement new code\. This section shows how to run commands or scripts on an instance\. 

**Topics**
+ [Running Commands](#cookbooks-101-basics-commands-script)
+ [Running Scripts](#cookbooks-101-basics-commands-execute)

## Running Commands<a name="cookbooks-101-basics-commands-script"></a>

The [https://docs.chef.io/chef/resources.html#script](https://docs.chef.io/chef/resources.html#script) resource runs one or more commands \. It supports the csh, bash, Perl, Python, and Ruby command interpreters, so it can be used on either Linux or Windows systems as long as they have the appropriate interpreters installed\. This topic shows how to run a simple bash command on a Linux instance\. Chef also supports [powershell\_script](https://docs.chef.io/chef/resources.html#powershell-script) and [batch](https://docs.chef.io/chef/resources.html#batch) resources to run scripts on Windows\. For more information, see [Running a Windows PowerShell Script](cookbooks-101-opsworks-opsworks-powershell.md)\. 

**To get started**

1. Inside the `opsworks_cookbooks` directory, create a directory named `script` and navigate to it\.

1. Add a `metadata.rb` file to `script` with the following content\.

   ```
   name "script"
   version "0.1.0"
   ```

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md), and remove CentOS from the `platforms` list\.

1. Inside `script`, create a directory named `recipes`\.

You can run commands by using the `script` resource itself, but Chef also supports a set of command interpreter\-specific versions of the resource, which are named for the interpreter\. The following recipe uses a [https://docs.chef.io/chef/resources.html#bash](https://docs.chef.io/chef/resources.html#bash) resource to run a simple bash script\.

```
bash "install_something" do
  user "root"
  cwd "/tmp"
  code <<-EOH
    touch somefile
  EOH
  not_if do
    File.exists?("/tmp/somefile")
  end
end
```

The `bash` resource is configured as follows\.
+ It uses the default action, `run`, which runs the commands in the `code` block\.

  This example has one command, `touch somefile`, but a `code` block can contain multiple commands\.
+ The `user` attribute specifies the user that executes the command\.
+ The `cwd` attribute specifies the working directory\.

  For this example, `touch` creates a file in the `/tmp` directory\.
+ The `not_if` guard attribute directs the resource to take no action if the file already exists\.

**To run the recipe**

1. Create a `default.rb` file that contains the preceding example code and save it to `recipes`\.

1. Run `kitchen converge`, then log in to the instance to verify that the file is in `/tmp`\.

## Running Scripts<a name="cookbooks-101-basics-commands-execute"></a>

The `script` resource is convenient, especially if you need to run only one or two commands, but it's often preferable to store the script in a file and execute the file\. The [https://docs.chef.io/chef/resources.html#execute](https://docs.chef.io/chef/resources.html#execute) resource runs a specified executable file, including script files, on Linux or Windows\. This topic modifies the `script` cookbook from the preceding example to use `execute` to run a simple shell script\. You can easily extend the example to more complex scripts, or other types of executable file\.

**To set up the script file**

1. Add a `files` subdirectory to `script` and a `default` subdirectory to `files`\.

1. Create a file named `touchfile` that contains the following and add it to `files/default`\. A common Bash interpreter line is used in this example, but substitute an interpreter that works for your shell environment if necessary\.

   ```
   #!/usr/bin/env bash
   touch somefile
   ```

   The script file can contain any number of commands\. For convenience, this example script has only a single `touch` command\.

The following recipe executes the script\. 

```
cookbook_file "/tmp/touchfile" do
  source "touchfile"
  mode 0755
end

execute "touchfile" do
  user "root"
  cwd "/tmp"
  command "./touchfile"
end
```

The `cookbook_file` resource copies the script file to `/tmp` and sets the mode to make the file executable\. The `execute` resource then executes the file as follows:
+ The `user` attribute specifies the command's user \(`root` in this example\)\.
+ The `cwd` attribute specifies the working directory \(`/tmp` in this example\)\.
+ The `command` attribute specifies the script to be executed \(`touchfile` in this example\), which is located in the working directory\.

**To run the recipe**

1. Replace the code in `recipes/default.rb` with the preceding example\.

1. Run `kitchen converge`, then log in to the instance to verify that `/tmp` now contains the script file, with the mode set to 0755, and `somefile`\.

When you are finished, run `kitchen destroy` to shut down the instance\. The next section uses a new cookbook\.