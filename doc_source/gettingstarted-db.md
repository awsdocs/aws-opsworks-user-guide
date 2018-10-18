# Step 3: Add a Back\-end Data Store<a name="gettingstarted-db"></a>

[Step 2\.1: Create a Stack \- Chef 11](gettingstarted-simple-stack.md) showed you how to create a stack that served a PHP application\. However, that was a very simple application that did little more than display some static text\. Production applications commonly use a back\-end data store, yielding a stack configuration something like the illustration that follows\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/php_walkthrough_arch_3.png)

This section shows how to extend MyStack to include a back\-end MySQL database server\. You need to do more than just add a MySQL server to the stack, though\. You also have to configure the app to communicate properly with the database server\. AWS OpsWorks Stacks doesn't do this for you; you will need to implement some custom recipes to handle that task\.

**Topics**
+ [Step 3\.1: Add a Back\-end Database](gettingstarted-db-db.md)
+ [Step 3\.2: Update SimplePHPApp](gettingstarted-db-update.md)
+ [A Short Digression: Cookbooks, Recipes, and AWS OpsWorks Stacks Attributes](gettingstarted-db-recipes.md)
+ [Step 3\.3: Add the Custom Cookbooks to MyStack](gettingstarted-db-cookbooks.md)
+ [Step 3\.4: Run the Recipes](gettingstarted-db-lifecycle.md)
+ [Step 3\.5: Deploy SimplePHPApp, Version 2](gettingstarted-db-deploy.md)
+ [Step 3\.6: Run SimplePHPApp](gettingstarted-db-run.md)