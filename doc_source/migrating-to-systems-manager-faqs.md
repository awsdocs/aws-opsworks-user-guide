# FAQ<a name="migrating-to-systems-manager-faqs"></a>

The following FAQs provide answers to some common questions\.

**Topics**
+ [Which AWS OpsWorks Stacks versions can I migrate?](#w2ab1c14c39c17b7)
+ [Which Chef versions can my migrated instances use?](#w2ab1c14c39c17b9)
+ [Which repository types can I migrate?](#w2ab1c14c39c17c11)
+ [Can I continue using a private Git repository?](#w2ab1c14c39c17c13)
+ [What SSH keys can I use to access my instances?](#w2ab1c14c39c17c15)
+ [Why are my instances automatically scaling in and out?](#w2ab1c14c39c17c17)
+ [Can I turn off Auto Scaling?](#w2ab1c14c39c17c19)
+ [Can I perform kernel and package updates on launched EC2 instances?](#w2ab1c14c39c17c21)
+ [Why don't the EBS volumes in my instances contain any data?](#w2ab1c14c39c17c23)
+ [Why aren't the EBS volumes described in my launch template mounted?](#w2ab1c14c39c17c25)
+ [Where can I find Chef recipe and Mount EBS volume logs?](#w2ab1c14c39c17c27)
+ [Where can I find the debug log for the migration script?](#w2ab1c14c39c17c29)
+ [Does the migration script support CloudFormation template versioning?](#w2ab1c14c39c17c31)
+ [Can I migrate multiple layers?](#w2ab1c14c39c17c33)
+ [How do I create a `SecureString` parameter?](#w2ab1c14c39c17c35)
+ [How can I protect instances in the new Auto Scaling group from termination events?](#w2ab1c14c39c17c37)
+ [What load balancers are available with the migration script?](#w2ab1c14c39c17c39)
+ [Are custom cookbook configure recipes migrated?](#w2ab1c14c39c17c41)
+ [Can I run deploy and undeploy recipes on my newly created instances?](#w2ab1c14c39c17c43)
+ [Can I change what subnets my Auto Scaling group spans?](#w2ab1c14c39c17c45)

## Which AWS OpsWorks Stacks versions can I migrate?<a name="w2ab1c14c39c17b7"></a>

 You can only migrate Chef 11\.10 and Chef 12, Amazon Linux, Amazon Linux 2, Ubuntu, and Red Hat Enterprise Linux 7 stacks\. 

## Which Chef versions can my migrated instances use?<a name="w2ab1c14c39c17b9"></a>

 Migrated instances can use Chef versions 11 through 14\. 

**Note**  
Windows stack migration is not supported\.

## Which repository types can I migrate?<a name="w2ab1c14c39c17c11"></a>

 You can migrate S3, Git, and HTTP repository types\. 

## Can I continue using a private Git repository?<a name="w2ab1c14c39c17c13"></a>

Yes, you can continue to use a private Git repository\.

If you use a private GitHub repository, you must create a new `Ed25519` host key for SSH\. This is because GitHub changed which keys are supported in SSH and removed the unencrypted Git protocol\. For more information about the `Ed25519` host key, see the GitHub blog post [Improving Git protocol security on GitHub](https://github.blog/2021-09-01-improving-git-protocol-security-github/)\. After you generate a new `Ed25519` host key, create a Systems Manager `SecureString` parameter for this SSH key and use the parameter name as the value for the `--repo-private-key` parameter\. For more information about how to create a Systems Manager `SecureString` parameter, see [Create a SecureString parameter \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/param-create-cli.html#param-create-cli-securestring) in the *AWS Systems Manager User Guide*\.

For any other Git repository type, create a Systems Manager `SecureString` parameter for this SSH key and use the parameter name as the value for the script's `--repo-private-key` parameter\.

## What SSH keys can I use to access my instances?<a name="w2ab1c14c39c17c15"></a>

When you run the script, the script migrates the SSH keys and instances configured in the stack\. You can use the SSH keys to access your instance\. If SSH keys are provided for the stack and instance, the script uses the keys from the stack\. If you are not sure which SSH keys to use, view the instances in the [EC2 console](Amazon EC2 console)\. The **Details** page in the EC2 console shows the SSH keys for your instance\.

## Why are my instances automatically scaling in and out?<a name="w2ab1c14c39c17c17"></a>

Auto Scaling scales instances based on the scaling rules for the Auto Scaling group\. You can set the **Min**, **Max**, and **Desired capacity** values for your group\. The Auto Scaling group automatically scales your capacity accordingly when you update these values\.

## Can I turn off Auto Scaling?<a name="w2ab1c14c39c17c19"></a>

You can turn off Auto Scaling by setting the Auto Scaling group's **Min**, **Max**, and **Desired capacity** values to the same number\. For example, if you want to always have ten instances, set the **Min**, **Max**, and **Desired capacity** values to ten\. 

## Can I perform kernel and package updates on launched EC2 instances?<a name="w2ab1c14c39c17c21"></a>

 By default, kernel and packages updates occur when the EC2 instance boots\. Use the following steps to perform kernel or package updates on a launched EC2 instance\. For example, you may want to apply updates after running deploy or configure recipes\. 

1.  Connect to your EC2 instance\. 

1.  Create the following `perform_upgrade` function and run it on your instance\. 

   ```
   perform_upgrade() {
       #!/bin/bash
       if [ -e '/etc/system-release' ] || [ -e '/etc/redhat-release' ]; then
        sudo yum -y update
       elif [ -e '/etc/debian_version' ]; then
        sudo apt-get update
        sudo apt-get dist-upgrade -y
       fi
   }
   perform_upgrade
   ```

1.  After the kernel and package updates, you may need to reboot your EC2 instance\. To check whether you a reboot is required, create the following `reboot_if_required` function and run it on your EC2 instance\. 

   ```
   reboot_if_required () {
    #!/bin/bash
    if [ -e '/etc/debian_version' ]; then
      if [ -f /var/run/reboot-required ]; then
        echo "reboot is required"
      else
        echo "reboot is not required"
      fi
    elif [ -e '/etc/system-release' ] || [ -e '/etc/redhat-release' ]; then
     export LC_CTYPE=en_US.UTF-8
     export LC_ALL=en_US.UTF-8
     LATEST_INSTALLED_KERNEL=`rpm -q --last kernel | perl -X -pe 's/^kernel-(\S+).*/$1/' | head -1`
     CURRENTLY_USED_KERNEL=`uname -r`
     if [ "${LATEST_INSTALLED_KERNEL}" != "${CURRENTLY_USED_KERNEL}" ];then
        echo "reboot is required"
     else
        echo "reboot is not required"
     fi
    fi
   }
   reboot_if_required
   ```

1.  If running the `reboot_if_required` results in a `reboot is required` message, reboot the EC2 instance\. If you receive a `reboot is not required` message, you do not need to reboot the EC2 instance\. 

## Why don't the EBS volumes in my instances contain any data?<a name="w2ab1c14c39c17c23"></a>

When you run the script, the script migrates the configuration of the EBS volumes, creating a replacement architecture for your OpsWorks stack and layer\. The script does not migrate actual instances or the data contained in the instances\. The script only migrates the configuration of EBS volumes at the layer level and attaches the empty EBS volumes to launched EC2 instances\.

Take the following steps to pull data from your previous instances' EBS volumes\.

1. Take a snapshot of your previous instances EBS volumes\. For more information about creating a snapshot, see [Create Amazon EBS snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-snapshot.html) in the *Amazon EC2 User Guide*\.

1. Create a volume from your snapshot\. For more information about creating a volume from a snapshot, see [Create a volume from a snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-volume.html#ebs-create-volume-from-snapshot) in the *Amazon EC2 User Guide*\.

1. Attach the volume you created to the instances\. For more information about attaching volumes, see [Attach an Amazon EBS volume to an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) in the *Amazon EC2 User Guide*\.

## Why aren't the EBS volumes described in my launch template mounted?<a name="w2ab1c14c39c17c25"></a>

 If you provide a launch template ID for the `--launch-template` parameter with EBS volumes, the script attaches the EBS volumes, but does not mount the volumes\. You can mount the attached EBS volumes by running the `MountEBSVolumes` RunCommand document that the script created for the launched EC2 instance\. 

 If you don't set `--launch-template` parameter, the script creates a template, and when the Auto Scaling group launches a new EC2 instance, the Auto Scaling group automatically attaches the EBS volumes and then runs the `SetupAutomation` command to mount the attached volumes to the mount points configured in the layer settings\. 

## Where can I find Chef recipe and Mount EBS volume logs?<a name="w2ab1c14c39c17c27"></a>

OpsWorks delivers the logs to an S3 bucket that you can specify by providing a value for the `--command-logs-bucket` parameter\. The default S3 bucket name has the format: `aws-opsworks-stacks-application-manager-logs-account-id`\. Chef recipe logs are stored in the `ApplyChefRecipes` prefix\. Mount EBS volume logs are stored in the `MountEBSVolumes` prefix\. All layers that are migrated from a stack deliver logs to the same S3 bucket\.

**Note**  
The S3 bucket's Lifecycle configuration includes a rule to delete the logs after 30 days\. If you want to keep the logs for more than 30 days, you must update the rule in the S3 bucket's Lifecycle configuration\.
Currently, OpsWorks only logs Chef `setup` and `terminate` recipes\.

## Where can I find the debug log for the migration script?<a name="w2ab1c14c39c17c29"></a>

The script places debug logs in a bucket named `aws-opsworks-stacks-transition-logs-account-id`\. You can find the debug logs in the `migration_script` folder of the S3 bucket under folders that match the name of the layer you migrated\.

## Does the migration script support CloudFormation template versioning?<a name="w2ab1c14c39c17c31"></a>

The script generates Systems Manager documents of type CloudFormation that create a replacement for the layer or stack you want to migrate\. Running the script again, even with the same parameters, exports a new version of the previously\-exported layer template\. The template versions are stored in the same S3 bucket as the script logs\.

## Can I migrate multiple layers?<a name="w2ab1c14c39c17c33"></a>

The script's `--layer-id` parameter passes in a single layer\. To migrate multiple layers, rerun the script and pass in a different `--layer-id`\.

Layers that are part of the same OpsWorks stack are listed under the same application in Application Manager\.

1.  Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. 

1. In the navigation pane, choose **Application Manager**\.

1.  In the **Applications** section, choose **Custom applications**\. 

1.  Choose your application\. The application name begins with `app-stack-name-first-six-characters-stack-id`\. 

1.  The top level element starting with app, shows all components which correspond to your OpsWorks stack\. This includes components corresponding to your OpsWorks layer\.

1.  Choose the component corresponding to the layer to view the resources for the layer\. The components representing OpsWorks layers are also visible from the **Custom applications** section as individual applications\.

## How do I create a `SecureString` parameter?<a name="w2ab1c14c39c17c35"></a>

You can use Systems Manager to create a `SecureString` parameter\. For more information about how to create a Systems Manager `SecureString` parameter, see [Create a SecureString parameter \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/param-create-cli.html#param-create-cli-securestring) or [Create a Systems Manager parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-create-console.html) in the *AWS Systems Manager User Guide*\.

You must provide a `SecureString` parameter as the value for the `--http-username`, `--http-password`, or `--repo-private-key` parameters\.

## How can I protect instances in the new Auto Scaling group from termination events?<a name="w2ab1c14c39c17c37"></a>

You can protect instances by setting the `--enable-instance-protection` parameter to `TRUE` and adding a `protected_instance` tag key to each EC2 instance you want to protect from termination events\. When you set the `--enable-instance-protection` parameter to `TRUE` and add a `protected_instance` tag key, the script adds a custom termination policy to your new Auto Scaling group and suspends the `ReplaceUnhealthy` process\. Instances with the `protected_instance` tag key are protected from the following termination events: 
+ Scale in events
+ Instance refresh
+ Rebalancing
+ Instance max lifetime
+ Allow listing instance termination
+ Termination and replacement of unhealthy instances

**Note**  
You must set the `protected_instance` tag key on instances you want to protect\. The tag key is case sensitive\. Any instance with that tag key is protected regardless of the tag value\.  
 To reduce the run time of the custom termination policy, you can increase the default number of instances the Lambda function uses to filter for protected instances by updating the value for the `default_sample_size` function code variable\. The default value is 15\. If you increase the `default_sample_size`, you may need to increase the memory allocated to the Lambda function, which would increase the cost of your Lambda function\. For information about AWS Lambda pricing, see [AWS Lambda Pricing](http://aws.amazon.com/)\.

## What load balancers are available with the migration script?<a name="w2ab1c14c39c17c39"></a>

The script provides three load balancer options\.
+  \(Recommended\) Create a new Application Load Balancer\. By default, the script creates a new Application Load Balancer\. You can also set the `--lb-type` parameter to `ALB`\. For more information about Application Load Balancers, see [What is an Application Load Balancer?](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) in the *Elastic Load Balancing User Guide*\. 
+  If an Application Load Balancer is not an option, create a Classic Load Balancer by setting the `--lb-type` parameter to `Classic`\. If you select this option, your existing Classic Load Balancer attached to your OpsWorks layer is kept separate from your application\. For more information about Application Load Balancers, see [What is a Classic Load Balancer?](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/introduction.html) in the *Elastic Load Balancing: Classic Load Balancers User Guide*\. 
+  You can attach an existing load balancer by setting the `--lb-type` parameter to `None`\. 
**Important**  
 We recommend creating new Elastic Load Balancing load balancers for your AWS OpsWorks Stacks layers\. If you choose to use an existing Elastic Load Balancing load balancer, you should first confirm that it is not being used for other purposes and has no attached instances\. After the load balancer is attached to the layer, OpsWorks removes any existing instances and configures the load balancer to handle only the layer's instances\. Although it is technically possible to use the Elastic Load Balancing console or API to modify a load balancer's configuration after attaching it to a layer, you should not do so; the changes will not be permanent\. 

**To attach your existing OpsWorks layer load balancer to your Auto Scaling group**

1. Run the migration script with the `--lb-type` parameter set to `None`\. When the value is set to `None`, the script does not clone or create a load balancer\.

1. After the script deploys the CloudFormation stack, update the Auto Scaling groups `Min` `Max` and `Desired capacity` values, and then test your application\.

1. Choose `Link to the template` shown in the script's output\. If you closed your terminal, take these steps to access the template\.

   1.  Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. 

   1. In the navigation pane, choose **Application Manager**\.

   1. Choose **CloudFormation stacks** and then choose **Template library**\.

   1. Choose **Owned by me** and locate your template\.

1. From the CloudFormation template, choose **Edit** from the **Actions** menu\.

1. Update the `LabelBalancerNames` property within the `ApplicationAsg` resource section of the CloudFormation template\.

   ```
   ApplicationAsg:
      DependsOn: CustomTerminationLambdaPermission
      Properties:
      #(other properties in ApplicationAsg to remain unchanged)
         LoadBalancerNames:
           - load-balancer-name 
         HealthCheckType: ELB
   ```

1. If you want your Auto Scaling group instances health check to also use the load balancer's health check, remove the section below `HealthCheckType` and enter `ELB`\. If you only require EC2 health checks, you do not need to change the template\. 

1. Save your changes\. Saving creates a new default version of the template\. If this is the first time you've run the script for the layer and the first time you've saved changes in the console, the newer version is 2\.

1. From **Actions **, choose **Provision stack**\. 

1. Confirm that you want to use the default version of the template\. Be sure **Select an existing stack** is selected and choose the CloudFormation stack to update\.

1. Choose **Next** for each of the subsequent pages until you see the **Review and Provision** page\. On the **Review and Provision** page, choose both **I acknowledge that AWS CloudFormation might create IAM resources with custom names** and **I understand that changes in the selected template can cause AWS CloudFormation to update or remove existing AWS resources\.**

1. Choose **Provision stack**\.

If you need to roll back your updates, take the following steps\.

1. Choose **Actions** and then choose **Provision stack**\.

1. Choose **Pick one of the existing versions** and then choose the previous template version\. 

1. Choose **Select an existing stack** and then choose the CloudFormation stack to update\.

## Are custom cookbook configure recipes migrated?<a name="w2ab1c14c39c17c41"></a>

Configure custom cookbooks are not supported to run during a setup event\. The script migrates custom cookbook configure recipes and creates a Systems Manager Automation runbook for you\. However, you must run the recipes manually\.

Take the following steps to run your configure recipes\.

1.  Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. 

1. In the navigation pane, choose **Application Manager**\.

1.  In the **Applications** section, choose **Custom applications**\. 

1.  Choose your application\. The application name begins with `app-stack-name`\. 

1.  Choose **Resources** and then choose the configure runbook\.**** 

1. Choose **Execute Automation**\.

1.  Choose the instance IDs for which you want to run the configure recipes and then choose **Execute**\. 

## Can I run deploy and undeploy recipes on my newly created instances?<a name="w2ab1c14c39c17c43"></a>

The script can create three possible Automation runbooks depending on the configuration of your layer\.
+  Setup 
+  Configure 
+  Terminate 

The script can also create the following Systems Manager parameters that contain input values for the `AWS-ApplyChefRecipes Run Command` document\.
+  Setup 
+  Deploy 
+  Configure 
+  Undeploy 
+  Terminate 

When a scale\-out event happens, the setup Automation runbook runs automatically\. This includes the setup and deploy custom cookbook recipes from your original OpsWorks layer\. When a scale\-in event happens, the terminate Automation runbook runs automatically\. The terminate Automation runbook contains the shutdown recipes from your original OpsWorks layer\.

If you want to run undeploy or configure recipes manually, take the following steps\.

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1.  In the **Applications** section, choose **Custom applications**\. 

1.  Choose your application\. The application name begins with `app-stack-name-first-six-characters-stack-id`\. Application Manager opens the **Overview** tab\. 

1.  Choose **Resources** and then choose the configure Automation runbook\. 

1. Choose **Execute Automation**\.

1.  For the `applyChefRecipesPropertiesParameter` Automation runbook input parameter, reference the correct Systems Manager parameter\. The Systems Manager parameter name follows the format `/ApplyChefRecipes-Preset/OpsWorks-stack-name-OpsWorks-layer-name-first-six-characters-stack-id/event` , where the value for *event* is `Configure`, `Deploy`, or `Undeploy` depending on the recipes you want to run\. 

1. Choose the instance IDs where you want to run the recipes and choose **Execute**\.

## Can I change what subnets my Auto Scaling group spans?<a name="w2ab1c14c39c17c45"></a>

By default, the Auto Scaling group spans all subnets in your OpsWorks stack VPC\. To update which subnets to span, take the following steps\. 

1. Choose `Link to the template` shown in the script's output\. If you closed your terminal, take these steps to access the template\.

   1.  Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. 

   1. In the navigation pane, choose **Application Manager**\.

   1. Choose **CloudFormation stacks** and then choose **Template library**\.

   1. Choose **Owned by me** and locate your template\.

1.  From **Actions**, choose **Provision stack**\. 

1.  Confirm that you want to use the default template\. Choose **Select an existing stack** and then choose the CloudFormation stack to update\. 
**Note**  
 If you ran the script with the `--provision-application` parameter set to `FALSE`, you must create a new CloudFormation stack\. 

1.  For the `SubnetIDs` parameter, provide a comma separated list of the subnet IDs that you want your Auto Scaling group to span\. 

1.  Choose **Next** until you see the **Review and Provision** page\. 

1.  On the **Review and Provision** page, choose **I acknowledge that AWS CloudFormation might create IAM resources with custom names** and **I understand that changes in the selected template can cause AWS CloudFormation to update or remove existing AWS resources**\. 

1.  Choose **Provision stack**\. 