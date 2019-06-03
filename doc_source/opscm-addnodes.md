# Add Nodes for the Chef Server to Manage<a name="opscm-addnodes"></a>

The [https://docs.chef.io/chef_client.html](https://docs.chef.io/chef_client.html) agent runs Chef recipes on physical or virtual computers, called *nodes*, that are associated with the server\. You can connect on\-premises computers or instances to the Chef server to manage, provided the nodes are running supported operating systems\. Registering nodes with the Chef server installs the `chef-client` agent software on those nodes\.

The minimum supported version of `chef-client` on nodes associated with an AWS OpsWorks for Chef Automate server is 12\.16\.42\. We recommend running `chef-client` 14\.10\.9\.

This walkthrough demonstrates how to run a `knife` command that adds, or *bootstraps*, an EC2 instance so that the Chef server can manage it\. For more information about how to bootstrap nodes automatically by using a script to perform unattended association of nodes with the Chef server, see [Adding Nodes Automatically in AWS OpsWorks for Chef Automate](opscm-unattend-assoc.md)\.

## Supported Operating Systems<a name="w4ab1b9c23c13b9"></a>

For the current list of supported operating systems for nodes, see the [Chef website](https://docs.chef.io/platforms.html)\.

## Add Nodes with Knife<a name="w4ab1b9c23c13c11"></a>

The [https://github.com/chef/knife-ec2](https://github.com/chef/knife-ec2) plug\-in is included with the Chef DK\. If you are more familiar with `knife-ec2`, you can use it instead of `knife bootstrap` to provision and bootstrap new EC2instances\. Otherwise, launch a new EC2 instance, and then follow the steps in this section\.

**To add nodes to manage**

1. Run the following `knife bootstrap` command\. This command bootstraps an EC2 instance to the nodes that your Chef server will manage\. Note that you are instructing the Chef server to run recipes from the `nginx` cookbook that you installed in [Use Policyfile\.rb to Get Cookbooks from a Remote Source](opscm-starterkit.md#install-cookbooks-policyfile)\. For more information about adding nodes by running the `knife bootstrap` command, see [Bootstrap a Node](https://docs.chef.io/install_bootstrap.html) in the Chef documentation\.

   The following table shows valid user names for node operating systems in the `knife` command in this step\. If neither `root` nor `ec2-user` works, check with your AMI provider\. For more information about connecting to Linux\-based instances, see [Connecting to Your Linux Instance Using SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) in the AWS documentation\.  
**Valid values for user names in node operating systems**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opscm-addnodes.html)

   ```
   knife bootstrap INSTANCE_IP_ADDRESS -N INSTANCE_NAME -x USER_NAME --sudo --run-list "recipe[nginx]"
   ```

1. Verify that the new node was added by running the following commands, replacing *INSTANCE\_NAME* with the name of the instance that you just added\.

   ```
   knife client show INSTANCE_NAME
   knife node show INSTANCE_NAME
   ```

## More Info<a name="w4ab1b9c23c13c13"></a>

Visit the [Learn Chef tutorial site](https://learn.chef.io/tutorials/manage-a-node/opsworks) to learn more about using AWS OpsWorks for Chef Automate servers and Chef Automate premium features\.