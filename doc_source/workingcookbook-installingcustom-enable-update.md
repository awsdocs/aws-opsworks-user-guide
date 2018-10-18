# Updating Custom Cookbooks<a name="workingcookbook-installingcustom-enable-update"></a>

When you provide AWS OpsWorks Stacks with custom cookbooks, the built\-in Setup recipes create a local cache on each newly\-started instance, and download the cookbooks to the cache\. AWS OpsWorks Stacks then runs recipes from the cache, not the repository\. If you modify the custom cookbooks in the repository, you must ensure that the updated cookbooks are installed on your instances' local caches\. AWS OpsWorks Stacks automatically deploys the latest cookbooks to new instances when they are started\. For existing instances, however, the situation is different: 
+ You must manually deploy updated custom cookbooks to online instances\.
+ You do not have to deploy updated custom cookbooks to offline instance store\-backed instances, including load\-based and time\-based instances\.

  AWS OpsWorks Stacks automatically deploys the current cookbooks when the instances are restarted\. 
+ You must start offline EBS\-backed 24/7 instances that are not load\-based or time\-based\.
+ You cannot start offline EBS\-backed load\-based and time\-based instances, so the simplest approach is to delete the offline instances and add new instances to replace them\.

  Because they are now new instances, AWS OpsWorks Stacks automatically deploys the current custom cookbooks when the instances are started\.

**To manually update custom cookbooks**

1. Update your repository with the modified cookbooks\. AWS OpsWorks Stacks uses the cache URL that you provided when you originally installed the cookbooks, so the cookbook root file name, repository location, and access rights should not change\. 
   + For Amazon S3 or HTTP repositories, replace the original \.zip file with a new \.zip file that has the same name\.
   + For Git or Subversion repositories, [edit your stack settings](workingstacks-edit.md) to change the **Branch/Revision** field to the new version\.

1. On the stack's page, click **Run Command** and select the **Update Custom Cookbooks** command\.  
![\[Run Command page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/update_cookbooks.png)

1. Add a comment if desired\.

1. Optionally, specify a custom JSON object for the command to add custom attributes to the stack configuration and deployment attributes that AWS OpsWorks Stacks installs on the instances\. For more information, see [Using Custom JSON](workingstacks-json.md) and [Overriding Attributes](workingcookbook-attributes.md)\.

1. By default, AWS OpsWorks Stacks updates the cookbooks on every instance\. To specify which instances to update, select the appropriate instances from the list at the end of the page\. To select every instance in a layer, select the appropriate layer checkbox in the left column\.

1. Click **Update Custom Cookbooks** to install the updated cookbooks\. AWS OpsWorks Stacks deletes the cached custom cookbooks on the specified instances and installs the new cookbooks from the repository\.

**Note**  
This procedure is required only for existing instances, which have old versions of the cookbooks in their caches\. If you subsequently add instances to a layer, AWS OpsWorks Stacks deploys the cookbooks that are currently in the repository so they automatically get the latest version\.