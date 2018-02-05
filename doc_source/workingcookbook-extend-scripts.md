# Using Recipes to Run Scripts<a name="workingcookbook-extend-scripts"></a>

If you already have a script that performs the required customization tasks, the simplest approach to extending a layer is often to implement a simple recipe to run the script\. You can then assign the recipe to the appropriate lifecycle events, typically Setup or Deploy, or use the `execute_recipes` stack command to run the recipe manually\.

The following example runs a shell script on Linux instances, but you can use the same approach for other types of script, including Windows PowerShell scripts\.

```
cookbook_file "/tmp/lib-installer.sh" do
  source "lib-installer.sh"
  mode 0755
end

execute "install my lib" do
  command "sh /tmp/lib-installer.sh"
end
```

The `cookbook_file` resource represents a file that is stored in a subdirectory of a cookbook's `files` directory, and transfers the file to a specified location on the instance\. This example transfers a shell script, `lib-installer.sh`, to the instance's `/tmp` directory and sets the file's mode to `0755`\. For more information, see [cookbook\_file](https://docs.chef.io/chef/resources.html#cookbook-file)\.

The `execute` resource represents a command, such as a shell command\. This example runs `lib-installer.sh`\. For more information, see [execute](https://docs.chef.io/chef/resources.html#execute)\.

You can also run a script by incorporating it into a recipe\. The following example runs a bash script, but Chef also supports Csh, Perl, Python, and Ruby\.

```
script "install_something" do
  interpreter "bash"
  user "root"
  cwd "/tmp"
  code <<-EOH
    #insert bash script
  EOH
end
```

The `script` resource represents a script\. The example specifies a bash interpreter, sets user to `"root"`, and sets the working directory to `/tmp`\. It then runs the bash script in the `code` block, which can include as many lines as required\. For more information, see [script](https://docs.chef.io/chef/resources.html#script)\.

For more information on how to use recipes to run scripts, see [Example 7: Running Commands and Scripts](cookbooks-101-basics-commands.md)\. For an example of how to run a PowerShell script on a Windows instance, see [Running a Windows PowerShell Script](cookbooks-101-opsworks-opsworks-powershell.md)\.