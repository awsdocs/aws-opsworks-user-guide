# Extending a Layer<a name="workingcookbook-extend"></a>

Sometimes, you need to customize a built\-in layer beyond what can be handled by modifying AWS OpsWorks Stacks attributes or customizing templates\. For example, suppose you need to create symlinks, set file or folder modes, install additional packages, and so on\. You must extend custom layers to provide more than minimal functionality\. In that case, you will need to implement one or more custom cookbooks with recipes to handle the customization tasks\. This topic provides some examples of how to use recipes to extend a layer\.

If you are new to Chef, you should first read [Cookbooks 101](cookbooks-101.md), which is a tutorial that introduces the basics of how to implement cookbooks to perform a variety of common tasks\. For a detailed example of how to implement a custom layer, see [Creating a Custom Tomcat Server Layer](create-custom.md)\. 

**Topics**
+ [Using Recipes to Run Scripts](workingcookbook-extend-scripts.md)
+ [Using Chef Deployment Hooks](workingcookbook-extend-hooks.md)
+ [Running Cron Jobs on Linux Instances](workingcookbook-extend-cron.md)
+ [Installing and Configuring Packages on Linux Instances](workingcookbook-extend-package.md)