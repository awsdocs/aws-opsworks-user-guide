# Using Custom AMIs<a name="workinginstances-custom-ami"></a>

AWS OpsWorks Stacks supports two ways to customize instances: custom [Amazon Machine Images \(AMIs\)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) and Chef recipes\. Both approaches give you control over which packages and package versions are installed, how they are configured, and so on\. However, each approach has different advantages, so the best one depends on your requirements\.

The following are the primary reasons to consider using a custom AMI:
+ You want to prebundle specific packages instead of installing them after the instance boots\.
+ You want to control the timing of package updates to provide a consistent base image for your layer\.
+ You want instances—[load\-based](workinginstances-autoscaling.md) instances in particular—to boot as quickly as possible\. 

The following are the primary reasons to consider using Chef recipes:
+ They are more flexible than custom AMIs\.
+ They are easier to update\.
+ They can perform updates on running instances\. 

 In practice, the optimal solution might be a combination of both approaches\. For more information about recipes, see [Cookbooks and Recipes](workingcookbook.md)\.

**Topics**
+ [How Custom AMIs work with AWS OpsWorks Stacks](#workinginstances-custom-ami-work)
+ [Creating a Custom AMI for AWS OpsWorks Stacks](#workinginstances-custom-ami-create)

## How Custom AMIs work with AWS OpsWorks Stacks<a name="workinginstances-custom-ami-work"></a>

To specify a custom AMI for your instances, select **Use custom AMI** as the instance's operating system when you create a new instance\. AWS OpsWorks Stacks then displays a list of the custom AMIs in the stack's region and you select the appropriate one from the list\. For more information, see [Adding an Instance to a Layer](workinginstances-add.md)\.

**Note**  
You cannot specify a particular custom AMI as a stack's default operating system\. You can set `Use custom AMI` as the stack's default operating system, but you can specify a particular AMI only when you add new instances to layers\. For more information, see [Adding an Instance to a Layer](workinginstances-add.md) and [Create a New Stack](workingstacks-creating.md)\. While it might be possible to create instances with other operating systems \(such as CentOS 6\.*x*\) that have been created from custom or community\-generated AMIs, these are not officially supported\.

This topic discusses some general issues that you should consider before creating or using a custom AMI\.

**Topics**
+ [Startup Behavior](#workinginstances-custom-ami-work-startup)
+ [Choosing a Layer](#workinginstances-custom-ami-work-layer)
+ [Handling Applications](#workinginstances-custom-ami-work-apps)

### Startup Behavior<a name="workinginstances-custom-ami-work-startup"></a>

When you start the instance, AWS OpsWorks Stacks uses the specified custom AMI to launch a new Amazon EC2 instance\. AWS OpsWorks Stacks then uses [cloud\-init](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonLinuxAMIBasics.html#included-aws-command-line-tools) to install the AWS OpsWorks Stacks agent on the instance and the agent runs the instance's Setup recipes followed by the Deploy recipes\. After the instance is online, the agent runs the Configure recipes for every instance in the stack, including the newly added instance\.

### Choosing a Layer<a name="workinginstances-custom-ami-work-layer"></a>

The AWS OpsWorks Stacks agent usually does not conflict with installed packages\. However, the instance must be a member of at least one layer\. AWS OpsWorks Stacks always runs that layer's recipes, which could cause problems\. You should understand exactly what a layer's recipes do to an instance before adding an instance with a custom AMI to that layer\.

To see which recipes a particular layer type runs on your instance, open a stack that includes that layer\. Then click **Layers** in the navigation pane, and click **Recipes** for the layer of interest\. To see the actual code, click the recipe name\.

**Note**  
For Linux AMIs, one way to reduce the possibility of conflicts is to use AWS OpsWorks Stacks to provision and configure the instance that is the basis for your custom AMI\. For more information, see [Create a Custom Linux AMI from an AWS OpsWorks Stacks Instance](#workinginstances-custom-ami-create-opsworks)\.

### Handling Applications<a name="workinginstances-custom-ami-work-apps"></a>

In addition to packages, you might also want to include an application in the AMI\. If you have a large complex application, including it in the AMI can shorten the instance's startup time\. You can include small applications in your AMI, but there is usually little or no time advantage relative to having AWS OpsWorks Stacks deploy the application\. 

One option is to include the application in your AMI and also [create an app](workingapps-creating.md) that deploys the application to the instances from a repository\. This approach shortens your boot time but also provides a convenient way to update the application after the instance is running\. Note that Chef recipes are idempotent, so the deployment recipes won't modify the application as long as the version in the repository is the same as the one on the instance\.

## Creating a Custom AMI for AWS OpsWorks Stacks<a name="workinginstances-custom-ami-create"></a>

To use a custom AMI with AWS OpsWorks Stacks, you must first create an AMI from a customized instance\. You can choose from two basic approaches:
+ Use the Amazon EC2 console or API to create and customize an instance, based on a 64\-bit version of one of the [AWS OpsWorks Stacks\-supported AMIs](workinginstances-os.md)\.
+ For Linux AMIs, use OpsWorks to create an Amazon EC2 instance, based on the configuration of its associated layers\.

**Note**  
Be aware that an AMI might not work with all instance types, so make sure that your starting AMI is compatible with the instance types that you plan to use\. In particular, the [R3](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/r3-instances.html) instance types require a hardware\-assisted virtualization \(HVM\) AMI\. 

You then use the Amazon EC2 console or API to create a custom AMI from the customized instance\. You can use your custom AMIs in any stack that is in the same region by adding an instance to a layer and specifying your custom AMI\. For more information on how to create an instance that uses a custom AMI, see [Adding an Instance to a Layer](workinginstances-add.md)\.

**Note**  
By default, AWS OpsWorks Stacks installs all Amazon Linux updates on boot, which provides you with the latest release\. In addition, Amazon Linux releases a new version approximately every six months, which can involve significant changes\. By default, custom AMIs based on Amazon Linux are automatically updated to the new version when it is released\. The recommended practice is to lock your custom AMI to a specific Amazon Linux version, which allows you to defer the update until you have tested the new version\. For more information, see [How do I lock my AMI to a specific version?](http://aws.amazon.com/amazon-linux-ami/faqs/#lock)\.

**Topics**
+ [Create a Custom AMI using Amazon EC2](#workinginstances-custom-ami-create-ec2)
+ [Create a Custom Linux AMI from an AWS OpsWorks Stacks Instance](#workinginstances-custom-ami-create-opsworks)
+ [Create a Custom Windows AMI](#w4ab1c11c47c13c11c19c18)

### Create a Custom AMI using Amazon EC2<a name="workinginstances-custom-ami-create-ec2"></a>

The simplest way to create a custom AMI—and the only option for Windows AMIs—is to perform the entire task by using the Amazon EC2 console or API\. For more details about the following steps, see [Creating Your Own AMIs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami.html)\.

**To create a custom AMI using Amazon EC2 console or API**

1. Create an instance by using a 64\-bit version of one of the [AWS OpsWorks Stacks\-supported AMIs](workinginstances-os.md)\.

1. Customize the instance from Step 1 by configuring it, installing packages, and so on\. Remember that everything you install will be reproduced on every instance based on the AMI, so don’t include items that should be specific to a particular instance\.

1. Stop the instance and create a custom AMI\.

### Create a Custom Linux AMI from an AWS OpsWorks Stacks Instance<a name="workinginstances-custom-ami-create-opsworks"></a>

If you want to use a customized AWS OpsWorks Stacks Linux instance to create an AMI, you should be aware that every Amazon EC2 instance created by OpsWorks includes a unique identity\. If you create a custom AMI from such an instance, it will include that identity and all instances based on the AMI will have the same identity\. To ensure that the instances based on your custom AMI have a unique identity, you must remove the identity from the customized instance before creating the AMI\.

**To create a custom AMI from an AWS OpsWorks Stacks instance**

1. [Create a Linux stack](workingstacks-creating.md) and [add one or more layers](workinglayers-basics-create.md) to define the configuration of the customized instance\. You can use built\-in layers, customized as appropriate, as well as fully custom layers\. For more information, see [Customizing AWS OpsWorks Stacks](customizing.md)\.

1. [Edit the layers](workinglayers-basics-edit.md) and disable AutoHealing\.

1. [Add an instance with your preferred Linux distribution](workinginstances-add.md) to the layer or layers and [start it](workinginstances-starting.md)\. We recommend using an Amazon EBS\-backed instance\. Open the instance's details page and record its Amazon EC2 ID for later\.

1. When the instance is online, [log in with SSH](workinginstances-ssh.md), and perform one of the next four steps, depending upon your instance operating system\.

1. For an Amazon Linux instance in either a Chef 11 or Chef 12 stack, or a Red Hat Enterprise Linux 7 instance in a Chef 11 stack, do the following\.

   1. `sudo /etc/init.d/monit stop`

   1. `sudo /etc/init.d/opsworks-agent stop`

   1. `sudo rm -rf /etc/aws/opsworks/ /opt/aws/opsworks/ /var/log/aws/opsworks/ /var/lib/aws/opsworks/ /etc/monit.d/opsworks-agent.monitrc /etc/monit/conf.d/opsworks-agent.monitrc /var/lib/cloud/ /etc/chef`
**Note**  
For instances in a Chef 12 stack, add the following two folders to this command:  
`/var/chef`
`/opt/chef`

   1. `sudo rpm -e opsworks-agent-ruby`

   1. `sudo rpm -e chef`

1. For an Ubuntu 16\.04 LTS or 18\.04 LTS instance in a Chef 12 stack, do the following\.

   1. `sudo systemctl stop opsworks-agent`

   1. `sudo rm -rf /etc/aws/opsworks/ /opt/aws/opsworks/ /var/log/aws/opsworks/ /var/lib/aws/opsworks/ /etc/monit.d/opsworks-agent.monitrc /etc/monit/conf.d/opsworks-agent.monitrc /var/lib/cloud/ /var/chef /opt/chef /etc/chef`

   1. `sudo apt-get -y remove chef`

   1. `sudo dpkg -r opsworks-agent-ruby`

   1. `systemctl stop apt-daily.timer`

   1. `systemctl stop apt-daily-upgrade.timer`

   1. `rm /var/lib/systemd/timers/stamp-apt-daily.timer`

   1. `rm /var/lib/systemd/timers/stamp-apt-daily-upgrade.timer`

1. For other supported Ubuntu versions in a Chef 12 stack, do the following\.

   1. `sudo /etc/init.d/monit stop`

   1. `sudo /etc/init.d/opsworks-agent stop`

   1. `sudo rm -rf /etc/aws/opsworks/ /opt/aws/opsworks/ /var/log/aws/opsworks/ /var/lib/aws/opsworks/ /etc/monit.d/opsworks-agent.monitrc /etc/monit/conf.d/opsworks-agent.monitrc /var/lib/cloud/ /var/chef /opt/chef /etc/chef`

   1. `sudo apt-get -y remove chef`

   1. `sudo dpkg -r opsworks-agent-ruby`

1. For a Red Hat Enterprise Linux 7 instance in a Chef 12 stack, do the following\.

   1. `sudo systemctl stop opsworks-agent`

   1. `sudo rm -rf /etc/aws/opsworks/ /opt/aws/opsworks/ /var/log/aws/opsworks/ /var/lib/aws/opsworks/ /etc/monit.d/opsworks-agent.monitrc /etc/monit/conf.d/opsworks-agent.monitrc /var/lib/cloud/ /etc/chef /var/chef`

   1. `sudo rpm -e opsworks-agent-ruby`

   1. `sudo rpm -e chef`

1. This step depends on the instance type:
   + For an Amazon EBS\-backed instance, use the AWS OpsWorks Stacks console to [stop the instance ](workinginstances-starting.md) and create the AMI as described in [Creating an Amazon EBS\-Backed Linux AMI](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html)\.\.
   + For an instance store\-backed instance, create the AMI as described in [Creating an Instance Store\-Backed Linux AMI](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-instance-store.html) and then use the AWS OpsWorks Stacks console to stop the instance\.

     When you create the AMI, be sure to include the certificate files\. For example, you can call the [http://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/CLTRG-ami-bundle-vol.html](http://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/CLTRG-ami-bundle-vol.html) command with the `-i` argument set to `-i $(find /etc /usr /opt -name '*.pem' -o -name '*.crt' -o -name '*.gpg' | tr '\n' ',')`\. Do not remove the apt public keys when bundling\. The default `ec2-bundle-vol` command handles this task\.

1. Clean up your stack by returning to the AWS OpsWorks Stacks console and [deleting the instance](workinginstances-delete.md) from the stack\.

### Create a Custom Windows AMI<a name="w4ab1c11c47c13c11c19c18"></a>

The following procedures create custom AMIs for Windows Server 2012 R2\. You can choose other Windows Server operating systems in the Amazon EC2 management console; the oldest available release of Windows Server is Windows Server 2003 R2\.

**Important**  
Currently, the AWS OpsWorks Stacks agent cannot be installed on—and AWS OpsWorks Stacks cannot manage—Windows\-based instances that use a system UI language other than **English \- United States** \(en\-US\)\.

**Topics**
+ [Creating a Custom Windows AMI with `Sysprep`](#w4ab1c11c47c13c11c19c18c12)
+ [Creating a Custom Windows AMI Without `Sysprep`](#w4ab1c11c47c13c11c19c18c14)
+ [Adding a New Instance by Using a Custom Windows AMI](#w4ab1c11c47c13c11c19c18c16)

#### Creating a Custom Windows AMI with `Sysprep`<a name="w4ab1c11c47c13c11c19c18c12"></a>

Creating custom Windows AMIs by using Sysprep typically results in a slower instance launch, but a cleaner process\. The first\-time startup of an instance created from an image created with `Sysprep` takes more time because of `Sysprep` activities, restarts, AWS OpsWorks Stacks provisioning, and the first AWS OpsWorks Stacks run, including setup and configuration\. Complete the steps for creating a custom Windows AMI in the Amazon EC2 console\.

**To create a custom Windows AMI with Sysprep**

1. In the Amazon EC2 console, choose **Launch Instance**\.

1. Find **Microsoft Windows Server 2012 R2 Base**, and then choose **Select**\.

1. Choose the instance type that you want, and then choose **Configure Instance Details**\. Make configuration changes to the AMI, including machine name, storage, and security group settings\. Choose **Launch**\.

1. After the instance boot process finishes, get your password, and then connect to the instance in a Windows **Remote Desktop Connection** window\.

1. On the Windows **Start** screen, choose **Start**, and then begin typing **ec2configservice** until the results show the **EC2ConfigServiceSettings** console\. Open the console\.

1. On the **General** tab, make sure that the **Enable UserData execution** check box is filled \(although this option is not required for `Sysprep`, it is required for AWS OpsWorks Stacks to install its agent\)\. Clear the check box for the **Set the computer name of the instance\.\.\.** option, because this option can cause a restart loop with AWS OpsWorks Stacks\.

1. On the **Image** tab, set **Administrator Password** to either **Random** to allow Amazon EC2 to automatically generate a password that you can retrieve with an SSH key, or **Specify** to specify your own password\. `Sysprep` saves this setting\. If you specify your own password, store the password in a convenient place\. We recommend that you do not choose **Keep Existing**\.

1. Choose **Apply**, and then choose **Shutdown with Sysprep**\. When you are prompted to confirm, choose **Yes**\.

1. After the instance has stopped, in the Amazon EC2 console, right\-click the instance in the **Instances** list, choose **Image**, and then choose **Create Image**\.

1. On the **Create Image** page, provide a name and description for the image, and specify the volume configuration\. When you have finished, choose **Create Image**\.

1. Open the **Images** page, and wait for your image to change from the **pending** stage to **available**\. Your new AMI is ready to use\.

#### Creating a Custom Windows AMI Without `Sysprep`<a name="w4ab1c11c47c13c11c19c18c14"></a>

Complete the steps for creating a custom Windows AMI in the Amazon EC2 console\.

**To create a custom Windows AMI without Sysprep**

1. In the Amazon EC2 console, choose **Launch Instance**\.

1. Find **Microsoft Windows Server 2012 R2 Base**, and then choose **Select**\.

1. Choose the instance type that you want, and then choose **Configure Instance Details**\. Make configuration changes to the AMI, including machine name, storage, and security group settings\. Choose **Launch**\.

1. After the instance boot process finishes, get your password, and then connect to the instance in a Windows **Remote Desktop Connection** window\.

1. On the instance, open `C:\Program Files\Amazon\Ec2ConfigService\Settings\config.xml`, change the following two settings, and then save and close the file:
   + `Ec2SetPassword` to `Enabled`
   + `Ec2HandleUserData` to `Enabled`

1. Disconnect from the **Remote Desktop** session, and return to the Amazon EC2 console\.

1. In the **Instances** list, stop the instance\.

1. After the instance has stopped, in the Amazon EC2 console, right\-click the instance in the **Instances** list, choose **Image**, and then choose **Create Image**\.

1. On the **Create Image** page, provide a name and description for the image, and specify the volume configuration\. When you have finished, choose **Create Image**\.

1. Open the **Images** page, and wait for your image to change from the **pending** stage to **available**\. Your new AMI is ready to use\.

#### Adding a New Instance by Using a Custom Windows AMI<a name="w4ab1c11c47c13c11c19c18c16"></a>

After your image changes to the **available** state, you can create new instances that are based on your custom Windows AMI\. When you choose **Use custom Windows AMI** from the **Operating system** list, AWS OpsWorks Stacks displays a list of custom AMIs\.

**To add a new instance based on a custom Windows AMI**

1. When your new AMI is available, go to the AWS OpsWorks Stacks console, open the **Instances** page for a Windows stack, and choose **\+ Instance** near the bottom of the page to add a new instance\.

1. On the **New** tab, choose **Advanced**\.

1. On the **Operating system** drop\-down list, choose **Use custom Windows AMI**\.

1. On the **Custom AMI** drop\-down list, choose the AMI that you created, and then choose **Add Instance**\.

You can now start and run the instance\.