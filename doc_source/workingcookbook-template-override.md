# Extending AWS OpsWorks Stacks Configuration Files Using Custom Templates<a name="workingcookbook-template-override"></a>

**Note**  
Because AWS OpsWorks Stacks handles Chef runs differently for Windows stacks than for Linux stacks, you cannot use the techniques discussed in this section for Windows stacks\.

AWS OpsWorks Stacks uses templates to create files such as configuration files, which typically depend on attributes for many of the settings\. If you use custom JSON or custom cookbook attributes to override the AWS OpsWorks Stacks definitions, your preferred settings are incorporated into the configuration files in place of the AWS OpsWorks Stacks settings\. However, AWS OpsWorks Stacks does not necessarily specify an attribute for every possible configuration setting; it accepts the defaults for some settings and hardcodes others directly in the template\. You can't use custom JSON or custom cookbook attributes to specify preferred settings if there is no corresponding AWS OpsWorks Stacks attribute\.

You can extend the configuration file to include additional configuration settings by creating a custom template\. You can then add whatever configuration settings or other content you need to the file, and override any hardcoded settings\. For more information on templates, see [Templates](workingcookbook-installingcustom-components-templates.md)\.

**Note**  
You can override any built\-in template *except* opsworks\-agent\.monitrc\.erb\.

**To create a custom template**

1. Create a cookbook with the same structure and directory names as the built\-in cookbook\. Then, create a template file in the appropriate directory with the same name as the built\-in template that you want to customize\. For example, to use a custom template to extend the Apache `httpd.conf` configuration file, you must implement an `apache2` cookbook in your repository and your template file must be `apache2/templates/default/apache.conf.erb`\. Using exactly the same names allows AWS OpsWorks Stacks to recognize the custom template and use it instead of the built\-in template\.

   The simplest approach is to just copy the built\-in template file from the [built\-in cookbook's GitHub repository](https://github.com/aws/opsworks-cookbooks) to your cookbook and modify it as needed\. 
**Important**  
Do not copy any files from the built\-in cookbook except for the template files that you want to customize\. Copies of other types of cookbook file, such as recipes, create duplicate Chef resources and can cause errors\.

   The cookbook can also include custom attributes, recipes, and related files, but their file names should not duplicate built\-in file names\.

1. Customize the template file to produce a configuration file that meets your requirements\. You can add more settings, delete existing settings, replace hardcoded attributes, and so on\. 

1. If you haven't done so already, edit the stack settings to enable custom cookbooks and specify your cookbook repository\. For more information, see [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md)\.

**Note**  
For a complete walkthrough of this procedure, see [Overriding Built\-In Templates](cookbooks-101-opsworks-templates.md)\.

You don't have to implement any recipes or [add recipes to the layer configuration](workingcookbook-assigningcustom.md) to override a template\. AWS OpsWorks Stacks always runs the built\-in recipes\. When it runs the recipe that creates the configuration file, it will automatically use your custom template instead of the built\-in template\.

**Note**  
If AWS OpsWorks Stacks makes any changes to the built\-in template, your custom template might become out of sync and no longer work correctly\. For example, suppose your template refers to a dependent file, and the file name changes\. AWS OpsWorks Stacks doesn't make such changes often, and when a template does change, it lists the changes and gives you the option of upgrading to a new version\. You should monitor the AWS OpsWorks Stacks repository for changes, and manually update your template as needed\.