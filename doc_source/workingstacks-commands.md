# Run AWS OpsWorks Stacks Stack Commands<a name="workingstacks-commands"></a>

AWS OpsWorks Stacks provides a set of *stack commands*, which you can use to perform a variety of operations on a stack's instances\. To run a stack command, click **Run Command** on the **Stack** page\. You then choose the appropriate command, specify any options, and press the button at the lower right, which will be labeled with the command's name\. 

**Note**  
AWS OpsWorks Stacks also supports a set of *deployment commands*, which you use to manage app deployment\. For more information, see [Deploying Apps](workingapps-deploying.md)\.

You can run the following stack commands on any stack\.

**Update Custom Cookbooks**  
Updates the instances' custom cookbooks with the current version from the repository\. This command does not run any recipes\. To run the updated recipes, you can use an `Execute Recipes`, `Setup`, or `Configure` stack command, or [redeploy your application](workingapps-deploying.md) to run the Deploy recipes\. For more information on custom cookbooks, see [Cookbooks and Recipes](workingcookbook.md)\.

**Execute Recipes**  
Executes a specified set of recipes on the instances\. For more information, see [Manually Running Recipes](workingcookbook-manual.md)\.

**Setup**  
Runs the instances' Setup recipes\.

**Configure**  
Runs the instances' Configure recipes\.

**Note**  
To use **Setup** or **Configure** to run recipes on an instance, the recipes must be assigned to the corresponding lifecycle event for the instance's layer\. For more information, see [Executing Recipes](workingcookbook-executing.md)\.

You can run the following stack commands only on Linux\-based stacks\.

**Install Dependencies**  
Installs the instances' packages\. Starting in Chef 12, this command is not available\.

**Update Dependencies**  
\(Linux only\. Starting in Chef 12, this command is not available\.\) Installs regular operating system updates and package updates\. The details depend on the instances' operating system\. For more information, see [Managing Security Updates](workingsecurity-updates.md)\.  
Use the **Upgrade Operating System** command to upgrade instances to a new Amazon Linux version\.

**Upgrade Operating System**  
\(Linux only\) Upgrades the instances' Amazon Linux operating systems to the latest version\. For more information, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.   
After running **Upgrade Operating System**, we recommend that you also run **Setup**\. This ensures that services are correctly restarted\.

Stack commands have the following options, some of which appear only for certain commands\.

**Comment**  
\(Optional\) Enter any custom remarks you care to add\.

**Recipes to execute**  
\(Required\) This setting appears only if you select the **Execute Recipes** command\. Enter the recipes to be executed using the standard *cookbook\_name*::*recipe\_name* format, separated by commas\. If you specify multiple recipes, AWS OpsWorks Stacks executes them in the listed order\.

**Allow reboot**  
\(Optional\) This setting appears only if you select the **Upgrade Operating System** command\. The default value is **Yes**, which directs AWS OpsWorks Stacks to reboot the instances after installing the upgrade\.

**Custom Chef JSON**  
\(Optional\) Choose **Advanced** to display this option, which allows you to specify custom JSON attributes to be incorporated into the [stack configuration and deployment attributes](workingcookbook-json.md)\. 

**Instances**  
\(Optional\) Specify the instances on which to execute the command\. All online instances are selected by default\. To run the command on a subset of instances, select the appropriate layers or instances\. 

**Note**  
You might see execute\_recipes executions that you did not run listed on the **Deployment** and **Commands** pages\. This is usually the result of a permissions change, such as granting or removing SSH permissions for a user\. When you make such a change, AWS OpsWorks Stacks uses execute\_recipes to update permissions on the instances\.