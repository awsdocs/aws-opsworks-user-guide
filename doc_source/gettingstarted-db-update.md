# Step 3\.2: Update SimplePHPApp<a name="gettingstarted-db-update"></a>

To start, you need a new version of SimplePHPApp that uses a back\-end data store\. With AWS OpsWorks Stacks, it's easy to update an application\. If you use a Git or Subversion repository, you can have a separate repository branch for each app version\. The example app stores a version of the app that uses a back\-end database in the Git repository's version2 branch\. You just need to update the app's configuration to specify the new branch and redeploy the app\.

**To update SimplePHPApp**

1. 

**Open the App's Edit Page**

   In the navigation pane, click **Apps** and then click **edit** in the **SimplePHPApp** row's **Actions** column\.

1. 

**Update the App's Configuration**

   Change the following settings\.  
**Branch/Revision**  
This setting indicates the app's repository branch\. The first version of SimplePHPApp didn't connect to a database\. To use a the database\-enabled version of the app, set this value to **version2**\.  
**Document root**  
This setting specifies your app's root folder\. The first version of SimplePHPApp used the default setting, which installs `index.php` in the server's standard root folder \(`/srv/www` for PHP apps\)\. If you specify a subfolder here—just the name, no leading '/'—AWS OpsWorks Stacks appends it to the standard folder path\. Version 2 of SimplePHPApp should go in `/srv/www/web`, so set **Document root** to **web**\.  
**Data source type**  
This setting associates a database server with the app\. The example uses the MySQL instance that you created in the previous step, so set **Data source type** to OpsWorks and **Database instance** to the instance you created in the previous step, **db\-master1 \(mysql\)**\. Leave **Database name** empty; AWS OpsWorks Stacks will create a database on the server named with the app's short name, simplephpapp\.

   Then click **Save** to save the new configuration\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/gsb2.png)

1. Start the MySQL instance\.

After you update an app, AWS OpsWorks Stacks automatically deploys the new app version to any new app server instances when you start them\. However, AWS OpsWorks Stacks does not automatically deploy the new app version to existing server instances; you must do that manually, as described in [Step 2\.4: Create and Deploy an App \- Chef 11](gettingstarted-simple-app.md)\. You could deploy the updated SimplePHPApp now, but for this example, it's better to wait a bit\.