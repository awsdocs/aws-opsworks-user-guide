# Chef Logs<a name="troubleshoot-debug-log"></a>

Chef logs are one of your key troubleshooting resources, especially for debugging recipes\. AWS OpsWorks Stacks captures the Chef log for each command, and retains the logs for an instance's 30 most recent commands\. Because the run is in debug mode, the log contains a detailed description of the Chef run, including the text that is sent to `stdout` and `stderror`\. If a recipe fails, the log includes the Chef stack trace\.

AWS OpsWorks Stacks gives you several ways to view Chef logs\. Once you have the log information, you can use it to debug failed recipes\.

**Note**  
You can also view a specified log's tail by using SSH to connect to the instance and running the agent CLI `show_log` command\. For more information, see [Displaying Chef Logs](troubleshoot-debug-cli.md#troubleshoot-debug-cli-log)\.

**Topics**
+ [Viewing a Chef Log with the Console](#troubleshoot-debug-log-console)
+ [Viewing a Chef Log with the CLI or API](#troubleshoot-debug-log-cli)
+ [Viewing a Chef Log on an Instance](#troubleshoot-debug-log-instance)
+ [Interpreting a Chef Log](#troubleshoot-debug-log-interpret)
+ [Common Chef Log Errors](#troubleshoot-debug-log-errors)

## Viewing a Chef Log with the Console<a name="troubleshoot-debug-log-console"></a>

The simplest way to view a Chef log is to go to the instance's details page\. The **Logs** section includes an entry for each event and [Execute Recipes](workingstacks-commands.md) command\. The following shows an instance's **Logs** section, with **configure** and **setup** commands, which correspond to Configure and Setup lifecycle events\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gs11.png)

Click **show** in the appropriate command's **Log** column to view the corresponding Chef log\. If an error occurs, AWS OpsWorks Stacks automatically opens the log to the error, which is usually at the end of the file\.

## Viewing a Chef Log with the CLI or API<a name="troubleshoot-debug-log-cli"></a>

You can use the AWS OpsWorks Stacks CLI [http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-commands.html](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-commands.html) command or the [http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeCommands.html](http://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeCommands.html) API action to view the logs, which are stored on an Amazon S3 bucket\. The following shows how to use the CLI to view any of the current set of logs file for a specified instance\. The procedure for using `DescribeCommands` is essentially similar\.

**To use the AWS OpsWorks Stacks to view an instance's Chef logs**

1. Open the instance's details page and copy its **OpsWorks ID** value\.

1. Use the ID value to run the `describe-commands` CLI command, as follows:

   ```
   aws opsworks describe-commands --instance-id 67bf0da2-29ed-4217-990c-d895d51812b9
   ```

   The command returns a JSON object with an embedded object for each command that AWS OpsWorks Stacks has executed on the instance, with the most recent first\. The `Type` parameter contains the command type for each embedded object, a `configure` command, and a `setup` command in this example\.

   ```
   {
       "Commands": [
           {
               "Status": "successful",
               "CompletedAt": "2013-10-25T19:38:36+00:00",
               "InstanceId": "67bf0da2-29ed-4217-990c-d895d51812b9",
               "AcknowledgedAt": "2013-10-25T19:38:24+00:00",
               "LogUrl": "https://s3.amazonaws.com/prod_stage-log/logs/b6c402df-5c23-45b2-a707-ad20b9c5ae40?AWSAccessKeyId=AKIAIOSFODNN7EXAMPLE
   &Expires=1382731518&Signature=YkqS5IZN2P4wixjHwoC3aCMbn5s%3D&response-cache-control=private&response-content-encoding=gzip&response-content-
   type=text%2Fplain",
               "Type": "configure",
               "CommandId": "b6c402df-5c23-45b2-a707-ad20b9c5ae40",
               "CreatedAt": "2013-10-25T19:38:11+00:00",
               "ExitCode": 0
           },
           {
               "Status": "successful",
               "CompletedAt": "2013-10-25T19:31:08+00:00",
               "InstanceId": "67bf0da2-29ed-4217-990c-d895d51812b9",
               "AcknowledgedAt": "2013-10-25T19:29:01+00:00",
               "LogUrl": "https://s3.amazonaws.com/prod_stage-log/logs/2a90e862-f974-42a6-9342-9a4f03468358?AWSAccessKeyId=AKIAIOSFODNN7EXAMPLE
   &Expires=1382731518&Signature=cxKYHO8mCCd4MvOyFb6ywebeQtA%3D&response-cache-control=private&response-content-encoding=gzip&response-content-
   type=text%2Fplain",
               "Type": "setup",
               "CommandId": "2a90e862-f974-42a6-9342-9a4f03468358",
               "CreatedAt": "2013-10-25T19:26:01+00:00",
               "ExitCode": 0
           }
       ]
   }
   ```

1. Copy the `LogUrl` value to your browser to view the log\.

If the instance has more than a few commands, you can add parameters to `describe-commands` to filter which commands are included in the response object\. For more information, see [describe\-commands](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-commands.html)\.

## Viewing a Chef Log on an Instance<a name="troubleshoot-debug-log-instance"></a>

**Note**  
Topics in this section apply to Chef 12\. For information about the location of Chef logs for Chef 11\.10 and older releases, see [Troubleshooting Chef 11\.10 and Earlier Versions for Linux](http://docs.aws.amazon.com/opsworks/latest/userguide/troubleshooting-chef-11-linux.html)\.

### Linux instances<a name="troubleshoot-debug-log-instance-linux"></a>

AWS OpsWorks Stacks stores each instance's Chef logs in its `/var/chef/runs` directory\. \(For Linux instances, this directory also includes the associated [data bags](data-bags.md), stored as JSON\-formatted files\.\) You need [sudo privileges](opsworks-security-users.md) to access this directory\. The log for each run is in a file named `chef.log` inside of the individual run's subdirectory\.

AWS OpsWorks Stacks stores its internal logs in the instance's `/var/log/aws/opsworks` folder\. The information is usually not very helpful for troubleshooting purposes\. However, these logs are useful to AWS OpsWorks Stacks support, and you might be asked to provide them if you encounter an issue with the service\. The Linux logs can also sometimes provide useful troubleshooting data\.

### Windows instances<a name="troubleshoot-debug-log-instance-windows"></a>

**Agent Logs**  
On Windows instances, OpsWorks logs are stored in a `ProgramData` path such as the following\. The number includes a timestamp\. 

```
C:\ProgramData\OpsWorksAgent\var\logs\number
```

**Note**  
By default, `ProgramData` is a hidden folder\. To unhide it, navigate to **Folder Options**\. Under **View**, choose the option to show hidden files\.

The following example shows agent logs on a Windows instance\.

```
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         5/24/2015  11:59 PM     127277 command.20150524.txt
-a---         5/25/2015  11:59 PM     546772 command.20150525.txt
-a---         5/26/2015  11:59 PM     551514 command.20150526.txt
-a---         5/27/2015   9:43 PM     495181 command.20150527.txt
-a---         5/24/2015  11:59 PM      24353 keepalive.20150524.txt
-a---         5/25/2015  11:59 PM     106232 keepalive.20150525.txt
-a---         5/26/2015  11:59 PM     106208 keepalive.20150526.txt
-a---         5/27/2015   8:54 PM      92593 keepalive.20150527.txt
-a---         5/24/2015   7:19 PM       3891 service.20150524.txt
-a---         5/27/2015   8:54 PM       1493 service.20150527.txt
-a---         5/24/2015  11:59 PM     112549 wire.20150524.txt
-a---         5/25/2015  11:59 PM     501501 wire.20150525.txt
-a---         5/26/2015  11:59 PM     499640 wire.20150526.txt
-a---         5/27/2015   8:54 PM     436870 wire.20150527.txt
```

**Chef Logs**  
On Windows instances, Chef logs are stored in a `ProgramData` path such as the following\. The number includes a timestamp\.

```
C:\ProgramData\OpsWorksAgent\var\commands\number
```

**Note**  
This directory contains only the output of the first \(OpsWorks owned\) Chef run\.

The following example shows OpsWorks owned Chef logs on a Windows instance\.

```
 Mode                LastWriteTime            Name
 ----                -------------            ----
 d----         5/24/2015   7:23 PM            configure-7ecb5f47-7626-439b-877f-5e7cb40ab8be
 d----         5/26/2015   8:30 PM            configure-8e74223b-d15d-4372-aeea-a87b428ffc2b
 d----         5/24/2015   6:34 PM            configure-c3980a1c-3d08-46eb-9bae-63514cee194b
 d----         5/26/2015   8:32 PM            grant_remote_access-70dbf834-1bfa-4fce-b195-e50e85402f4c
 d----         5/26/2015  10:30 PM            revoke_remote_access-1111fce9-843a-4b27-b93f-ecc7c5e9e05b
 d----         5/24/2015   7:21 PM            setup-754ec063-8b60-4cd4-b6d7-0e89d7b7aa78
 d----         5/26/2015   8:27 PM            setup-af5bed36-5afd-4115-af35-5766f88bc039
 d----         5/24/2015   6:32 PM            setup-d8abeffa-24d4-414b-bfb1-4ad07319f358
 d----         5/24/2015   7:13 PM            shutdown-c7130435-9b5c-4a95-be17-6b988fc6cf9a
 d----         5/26/2015   8:25 PM            sync_remote_users-64c79bdc-1f6f-4517-865b-23d2def4180c
 d----         5/26/2015   8:48 PM            update_custom_cookbooks-2cc59a94-315b-414d-85eb-2bdea6d76c6a
```

**User Chef Logs**  
The logs for your Chef runs can be found in files named `logfile.txt` in a folder that is named after the numbered Chef command, as in the following diagram\.

 C:/chef └── `runs` └── `command-12345` ├── `attribs.json` ├── `client.rb` └── `logfile.txt` 

## Interpreting a Chef Log<a name="troubleshoot-debug-log-interpret"></a>

The beginning of the log contains mostly internal Chef logging\.

```
# Logfile created on Thu Oct 17 17:25:12 +0000 2013 by logger.rb/1.2.6
[2013-10-17T17:25:12+00:00] INFO: *** Chef 11.4.4 ***
[2013-10-17T17:25:13+00:00] DEBUG: Building node object for php-app1.localdomain
[2013-10-17T17:25:13+00:00] DEBUG: Extracting run list from JSON attributes provided on command line
[2013-10-17T17:25:13+00:00] INFO: Setting the run_list to ["opsworks_custom_cookbooks::load", "opsworks_custom_cookbooks::execute"] from JSON
[2013-10-17T17:25:13+00:00] DEBUG: Applying attributes from json file
[2013-10-17T17:25:13+00:00] DEBUG: Platform is amazon version 2013.03
[2013-10-17T17:25:13+00:00] INFO: Run List is [recipe[opsworks_custom_cookbooks::load], recipe[opsworks_custom_cookbooks::execute]]
[2013-10-17T17:25:13+00:00] INFO: Run List expands to [opsworks_custom_cookbooks::load, opsworks_custom_cookbooks::execute]
[2013-10-17T17:25:13+00:00] INFO: Starting Chef Run for php-app1.localdomain
[2013-10-17T17:25:13+00:00] INFO: Running start handlers
[2013-10-17T17:25:13+00:00] INFO: Start handlers complete.
[2013-10-17T17:25:13+00:00] DEBUG: No chefignore file found at /opt/aws/opsworks/releases/20131015111601_209/cookbooks/chefignore no files will be ignored
[2013-10-17T17:25:13+00:00] DEBUG: Cookbooks to compile: ["gem_support", "packages", "opsworks_bundler", "opsworks_rubygems", "ruby", "ruby_enterprise", "dependencies", "opsworks_commons", "scm_helper", :opsworks_custom_cookbooks]
[2013-10-17T17:25:13+00:00] DEBUG: Loading cookbook gem_support's library file: /opt/aws/opsworks/releases/20131015111601_209/cookbooks/gem_support/libraries/current_gem_version.rb
[2013-10-17T17:25:13+00:00] DEBUG: Loading cookbook packages's library file: /opt/aws/opsworks/releases/20131015111601_209/cookbooks/packages/libraries/packages.rb
[2013-10-17T17:25:13+00:00] DEBUG: Loading cookbook dependencies's library file: /opt/aws/opsworks/releases/20131015111601_209/cookbooks/dependencies/libraries/current_gem_version.rb
[2013-10-17T17:25:13+00:00] DEBUG: Loading cookbook opsworks_commons's library file: /opt/aws/opsworks/releases/20131015111601_209/cookbooks/opsworks_commons/libraries/activesupport_blank.rb
[2013-10-17T17:25:13+00:00] DEBUG: Loading cookbook opsworks_commons's library file: /opt/aws/opsworks/releases/20131015111601_209/cookbooks/opsworks_commons/libraries/monkey_patch_chefgem_resource.rb
...
```

This part of the file is useful largely to Chef experts\. Note that the run list includes only two recipes, even though most commands involve many more\. These two recipes handle the task of loading and executing all the other built\-in and custom recipes\.

The most interesting part of the file is typically at the end\. If a run ends successfully, you should see something like the following:

```
...
[Tue, 11 Jun 2013 16:00:50 +0000] DEBUG: STDERR: 
[Tue, 11 Jun 2013 16:00:50 +0000] DEBUG: ---- End output of /sbin/service mysqld restart ----
[Tue, 11 Jun 2013 16:00:50 +0000] DEBUG: Ran /sbin/service mysqld restart returned 0
[Tue, 11 Jun 2013 16:00:50 +0000] INFO: service[mysql]: restarted successfully
[Tue, 11 Jun 2013 16:00:50 +0000] INFO: Chef Run complete in 84.07096 seconds
[Tue, 11 Jun 2013 16:00:50 +0000] INFO: cleaning the checksum cache
[Tue, 11 Jun 2013 16:00:50 +0000] DEBUG: removing unused checksum cache file /var/chef/cache/checksums/chef-file--tmp-chef-rendered-template20130611-4899-8wef7e-0
[Tue, 11 Jun 2013 16:00:50 +0000] DEBUG: removing unused checksum cache file /var/chef/cache/checksums/chef-file--tmp-chef-rendered-template20130611-4899-1xpwyb6-0
[Tue, 11 Jun 2013 16:00:50 +0000] DEBUG: removing unused checksum cache file /var/chef/cache/checksums/chef-file--etc-monit-conf
[Tue, 11 Jun 2013 16:00:50 +0000] INFO: Running report handlers
[Tue, 11 Jun 2013 16:00:50 +0000] INFO: Report handlers complete
[Tue, 11 Jun 2013 16:00:50 +0000] DEBUG: Exiting
```

**Note**  
You can use the agent CLI to display the log's tail during or after the run\. For more information, see [Displaying Chef Logs](troubleshoot-debug-cli.md#troubleshoot-debug-cli-log)\.

If a recipe fails, you should look for an ERROR\-level output, which will contain an exception followed by a Chef stack trace, such as the following:

```
...
Please report any problems with the /usr/scripts/mysqlbug script!

[  OK  ]
MySQL Daemon failed to start.
Starting mysqld:  [FAILED]STDERR: 130611 15:07:55 [Warning] The syntax '--log-slow-queries' is deprecated and will be removed in a future release. Please use '--slow-query-log'/'--slow-query-log-file' instead.
130611 15:07:56 [Warning] The syntax '--log-slow-queries' is deprecated and will be removed in a future release. Please use '--slow-query-log'/'--slow-query-log-file' instead.
---- End output of /sbin/service mysqld start ----

/opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/mixin/command.rb:184:in `handle_command_failures'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/mixin/command.rb:131:in `run_command'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/provider/service/init.rb:37:in `start_service'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/provider/service.rb:60:in `action_start'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource.rb:406:in `send'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource.rb:406:in `run_action'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/runner.rb:53:in `run_action'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/runner.rb:89:in `converge'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/runner.rb:89:in `each'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/runner.rb:89:in `converge'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource_collection.rb:94:in `execute_each_resource'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource_collection/stepable_iterator.rb:116:in `call'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource_collection/stepable_iterator.rb:116:in `call_iterator_block'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource_collection/stepable_iterator.rb:85:in `step'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource_collection/stepable_iterator.rb:104:in `iterate'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource_collection/stepable_iterator.rb:55:in `each_with_index'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/resource_collection.rb:92:in `execute_each_resource'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/runner.rb:84:in `converge'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/client.rb:268:in `converge'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/client.rb:158:in `run'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/application/solo.rb:190:in `run_application'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/application/solo.rb:181:in `loop'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/application/solo.rb:181:in `run_application'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/../lib/chef/application.rb:62:in `run'
  /opt/aws/opsworks/releases/20130605160141_122/vendor/bundle/ruby/1.8/gems/chef-0.9.15.5/bin/chef-solo:25
  /opt/aws/opsworks/current/bin/chef-solo:16:in `load'
  /opt/aws/opsworks/current/bin/chef-solo:16
```

The end of the file is the Chef stack trace\. You should also examine the output just before the exception, which often contains a system error such as `package not available` that can also be useful in determining the failure cause\. In this case, the MySQL daemon failed to start\.

## Common Chef Log Errors<a name="troubleshoot-debug-log-errors"></a>

The following are some common Chef log errors, and how to address them\.

Log could not be found  
At the start of a Chef run, instances receive a presigned Amazon S3 URL that lets you view the log on a webpage when the Chef run is finished\. Because this URL expires after two hours, there is no log uploaded to the Amazon S3 site if a Chef run takes longer than two hours, even if no problems occurred during the Chef run\. The command to create a log succeeds, but the log can be viewed only on the instance, not at the presigned URL\.

Log ends abruptly  
If a Chef log ends abruptly without either indicating success or displaying error information, you have probably encountered a low\-memory state that prevented Chef from completing the log\. Your best option is to try again with a larger instance\.

Missing cookbook or recipe  
If the Chef run encounters a cookbook or recipe that is not in the cookbook cache, you will see something like the following:  

```
DEBUG: Loading Recipe mycookbook::myrecipe via include_recipe  
ERROR: Caught exception during execution of custom recipe: mycookbook::myrecipe:
   Cannot find a cookbook named mycookbook; did you forget to add metadata to a cookbook?
```
This entry indicates that the `mycookbook` cookbook is not in the cookbook cache\. With Chef 11\.4, you can also encounter this error if you do not declare dependencies correctly in `metadata.rb`\.  
AWS OpsWorks Stacks runs recipes from the instance's cookbook cache\. It downloads cookbooks from your repository to this cache when the instance starts\. However, AWS OpsWorks Stacks does not automatically update the cache on an online instance if you subsequently modify the cookbooks in your repository\. If you have modified your cookbooks or added new cookbooks since starting the instance, take the following steps:  

1. Make sure that you have committed your changes to the repository\.

1. Run the [Update Cookbooks stack command](workingstacks-commands.md) to update the cookbook cache with the most recent version from the repository\.

Local command failure  
If a Chef `execute` resource fails to execute the specified command, you will see something like the following:  

```
DEBUG: ---- End output of ./configure --with-config-file-path=/ returned 2 
ERROR: execute[PHP: ./configure] (/root/opsworks-agent/site-cookbooks/php-fpm/recipes/install.rb line 48) had an error:
   ./configure --with-config-file-path=/
```
Scroll up in the log and you should see the command's `stderr` and `stdout` output, which should help you determine why the command failed\.

Package failure  
If a package installation fails, you will see something like the following:  

```
ERROR: package[zend-server-ce-php-5.3] (/root/opsworks-agent/site-cookbooks/zend_server/recipes/install.rb line 20)
   had an error: apt-get -q -y --force-yes install zend-server-ce-php-5.3=5.0.4+b17 returned 100, expected 0
```
Scroll up in the log and you should see the command's STDOUT and STDERROR output, which should help you determine why the package installation failed\.