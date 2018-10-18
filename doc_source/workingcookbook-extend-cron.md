# Running Cron Jobs on Linux Instances<a name="workingcookbook-extend-cron"></a>

A Linux cron job directs the cron daemon to run one or more commands on a specified schedule\. For example, suppose your stack supports a PHP e\-commerce application\. You can set up a cron job to have the server send you a sales report at a specified time every week\. For more information about cron, see [cron](http://en.wikipedia.org/wiki/Cron) on Wikipedia\. For more information about how to run a cron job directly on a Linux\-based computer or instance, see [What are cron and crontab, and how do I use them?](https://kb.iu.edu/d/afiz) on the Indiana University knowledge base website\.

Although you can manually set up `cron` jobs on individual Linux\-based instances by connecting to them with SSH, and editing their `crontab` entries, a key advantage of AWS OpsWorks Stacks is that you can direct it to run the task across an entire layer of instances\. The following procedure describes how to set up a `cron` job on a PHP App Server layer's instances, but you can use the same approach with any layer\.

**To set up a `cron` job on a layer's instances**

1. Implement a cookbook with a recipe with a `cron` resource that sets up the job\. The example assumes that the recipe is named `cronjob.rb`; the implementation details are described later\. For more information on cookbooks and recipes, see [Cookbooks and Recipes](workingcookbook.md)\.

1. Install the cookbook on your stack\. For more information, see [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)\.

1. Have AWS OpsWorks Stacks run the recipe automatically on the layer's instances by assigning it to the following lifecycle events\. For more information, see [Automatically Running Recipes](workingcookbook-assigningcustom.md)\.
   + **Setup** – Assigning `cronjob.rb` to this event directs AWS OpsWorks Stacks to run the recipe on all new instances\.
   + **Deploy** – Assigning `cronjob.rb` to this event directs AWS OpsWorks Stacks to run the recipe on all online instances when you deploy or redeploy an app to the layer\.

   You can also manually run the recipe on online instances by using the `Execute Recipes` stack command\. For more information, see [Run Stack Commands](workingstacks-commands.md)\.

The following is the `cronjob.rb` example, which sets up a cron job to run a user\-implemented PHP application once a week that collects the sales data from the server and mails a report\. For more examples of how to use a cron resource, see [cron](https://docs.chef.io/chef/resources.html#cron)\. 

```
cron "job_name" do
  hour "1"
  minute "10"
  weekday "6"
  command "cd /srv/www/myapp/current && php .lib/mailing.php"
end
```

`cron` is a Chef resource that represents a `cron` job\. When AWS OpsWorks Stacks runs the recipe on an instance, the associated provider handles the details of setting up the job\.
+ `job_name` is a user\-defined name for the `cron` job, such as `weekly report`\.
+ `hour`/`minute`/`weekday` specify when the commands should run\. This example runs the commands every Saturday at 1:10 AM\.
+ `command` specifies the commands to be run\.

  This example runs two commands\. The first navigates to the `/srv/www/myapp/current` directory\. The second runs the user\-implemented `mailing.php` application, which collects the sales data and sends the report\.

**Note**  
The `bundle` command does not work with `cron` jobs by default\. The reason is that AWS OpsWorks Stacks installs bundler in the `/usr/local/bin` directory\. To use `bundle` with a `cron` job, you must explicitly add the path `/usr/local/bin` to the cron job\. Also, because the $PATH environment variable may not expand in the `cron` job, a best practice is to explicitly add any necessary path information to the job without relying on expansion of the $PATH variable\. The following examples show two ways to use `bundle` in a `cron` job\.  

```
cron "my first task" do
  path "/usr/local/bin"
  minute "*/10"
  command "cd /srv/www/myapp/current && bundle exec my_command"
end
```

```
cron_env = {"PATH" => "/usr/local/bin"}
cron "my second task" do
  environment cron_env
  minute "*/10"
  command "cd /srv/www/myapp/current && /usr/local/bin/bundle exec my_command"
end
```

If your stack has multiple application servers, assigning `cronjob.rb` to the PHP App Server layer's lifecycle events might not be an ideal approach\. For example, the recipe runs on all of the layer's instances, so you will receive multiple reports\. A better approach is to use a custom layer to ensure that only one server sends a report\.

**To run a recipe on just one of a layer's instances**

1. Create a custom layer called, for example, PHPAdmin and assign `cronjob.rb` to its Setup and Deploy events\. Custom layers don't necessarily have to do very much\. In this case, PHPAdmin just runs one custom recipe on its instances\.

1. Assign one of the PHP App Server instances to AdminLayer\. If an instance belongs to more than one layer, AWS OpsWorks Stacks runs each layer's built\-in and custom recipes\.

Because only one instance belongs to the PHP App Server and PHPAdmin layers, `cronjob.rb` runs only on that instance and you receive just one report\.