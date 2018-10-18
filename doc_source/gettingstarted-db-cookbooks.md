# Step 3\.3: Add the Custom Cookbooks to MyStack<a name="gettingstarted-db-cookbooks"></a>

You store custom cookbooks in a repository, much like apps\. Each stack can have a repository that contains one set of custom cookbooks\. You then direct AWS OpsWorks Stacks to install your custom cookbooks on the stack's instances\.

1. Click **Stack** in the navigation pane to see the page for the current stack\.

1. Click **Stack Settings**, and then click **Edit**\. 

1. Modify the stack configuration as follows:
   + **Use custom Chef Cookbooks** – **Yes**
   + **Repository type** – **Git**
   + **Repository URL** – **git://github\.com/amazonwebservices/opsworks\-example\-cookbooks\.git**

1. Click **Save** to update the stack configuration\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gsb6.png)

AWS OpsWorks Stacks then installs the contents of your cookbook repository on all of the stack's instances\. If you create new instances, AWS OpsWorks Stacks automatically installs the cookbook repository\.

**Note**  
If you need to update any of your cookbooks, or add new cookbooks to the repository, you can do so without touching the stack settings\. AWS OpsWorks Stacks will automatically install the updated cookbooks on all new instances\. However, AWS OpsWorks Stacks does not automatically install updated cookbooks on the stack's online instances\. You must explicitly direct AWS OpsWorks Stacks to update the cookbooks by running the `Update Cookbooks` stack command\. For more information, see [Run Stack Commands](workingstacks-commands.md)\.