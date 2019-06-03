# Adding Nodes Automatically in AWS OpsWorks for Chef Automate<a name="opscm-unattend-assoc"></a>

This topic describes how to add Amazon Elastic Compute Cloud \(Amazon EC2\) nodes to your Chef server automatically\. In [Add Nodes for the Chef Server to Manage](opscm-addnodes.md), you learned how to use the [https://docs.chef.io/install_bootstrap.html#knife-bootstrap](https://docs.chef.io/install_bootstrap.html#knife-bootstrap) command to add one node at a time to your Chef server\. The code in the Starter Kit shows how to add nodes automatically using the unattended method\. The recommended method of unattended \(or automatic\) association of new nodes is to configure the [Chef Client Cookbook](https://supermarket.chef.io/cookbooks/chef-client)\. You can use the `userdata` script in the Starter Kit, and change either the `run_list` section of the userdata script, or your `Policyfile.rb` with the cookbooks you want to apply to your nodes\. Before you run the `chef-client` agent, install the Chef Client cookbook on your Chef server, and then install the `chef-client` agent in service mode with, for example, an HTTPD role, as shown in the following sample command\. 

```
chef-client -r "chef-client,role[httpd]"
```

To communicate with the Chef server, the `chef-client` agent software must have access to the public key of the client node\. You can generate a public\-private key pair in Amazon EC2, and then pass the public key to the AWS OpsWorks `associate-node` API call with the node name\. The script included in the Starter Kit gathers your organization name, server name, and server endpoint for you\. This ensures that the node is associated with the Chef server, and the `chef-client` agent software that runs on the node can communicate with the server after matching the private key\.

The minimum supported version of `chef-client` on nodes associated with an AWS OpsWorks for Chef Automate server is 13\.*x*\. We recommend running the [most current, stable `chef-client` version](https://downloads.chef.io/chef/stable)\.

For information about how to disassociate a node, see [Disassociate a Node from an AWS OpsWorks for Chef Automate Server](opscm-disassociate-node.md) in this guide, and [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) in the AWS OpsWorks for Chef Automate API documentation\.

## Supported Operating Systems<a name="w4ab1b9c33c13"></a>

For the current list of supported operating systems for nodes, see the [Chef website](https://docs.chef.io/platforms.html)\.

## Step 1: Create an IAM Role to Use as Your Instance Profile<a name="opscm-create-instance-profile"></a>

Create an AWS Identity and Access Management \(IAM\) role to use as your EC2 instance profile, and attach the following policy to the IAM role\. This policy allows the AWS OpsWorks for Chef Automate \(`opsworks-cm`\) API to communicate with the EC2 instance during node registration\. For more information about instance profiles, see [Using Instance Profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the Amazon EC2 documentation\. For information about how to create an IAM role, see [Creating an IAM Role in the Console](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#create-iam-role-console) in the Amazon EC2 documentation\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "opsworks-cm:AssociateNode",
                "opsworks-cm:DescribeNodeAssociationStatus",
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

AWS OpsWorks provides an AWS CloudFormation template that you can use to create the IAM role with the preceding policy statement\. The following AWS CLI command creates the instance profile role for you by using this template\. You can omit the `--region` parameter if you want to create the new AWS CloudFormation stack in your default region\.

```
aws cloudformation --region region ID create-stack --stack-name myChefAutomateinstanceprofile --template-url https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-nodes-roles.yaml --capabilities CAPABILITY_IAM
```

## Step 2: Install the Chef Client Cookbook<a name="w4ab1b9c33c17"></a>

If you have not done so already, follow the steps in [\(Alternate\) Use Berkshelf to Get Cookbooks from a Remote Source](opscm-starterkit.md#opscm-berkshelf) to ensure that your Berksfile or `Policyfile.rb` file references the Chef Client cookbook and installs the cookbook\.

## Step 3: Create Instances by Using an Unattended Association Script<a name="opscm-unattend-script"></a>

1. To create EC2 instances, you can copy `userdata` script from the Starter Kit to the `userdata` section of EC2 instance instructions, Amazon EC2 Auto Scaling group launch configurations, or an AWS CloudFormation template\. For more information about adding scripts to user data, see [Running Commands on Your Linux Instance at Launch](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) in the Amazon EC2 documentation\.

   This script runs the `opsworks-cm` API [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html) command to associate a new node with your Chef server\.

   By default, the name of the new registered node is the instance ID, but you can change the name by modifying the value of the `NODE_NAME` variable\. Because changing the organization name in the Chef console UI is currently not possible, leave `CHEF_ORGANIZATION` set to `default`\.

1. Follow the procedure in [Launching an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the EC2 documentation, with modifications here\. In the EC2 instance launch wizard, choose an Amazon Linux AMI\.

1. On the **Configure Instance Details** page, select the role you created in [Step 1: Create an IAM Role to Use as Your Instance Profile](#opscm-create-instance-profile), as your IAM role\.

1. In the **Advanced Details** area, upload the `userdata.sh` script that you created earlier in this procedure\.

1. No changes are needed on the **Add Storage** page\. Go on to **Add Tags**\.

1. On the **Configure Security Group** page, choose **Add Rule**, and then choose the type **HTTP** to open port numbers 443 and 80 for the Apache web server in this example\.

1. Choose **Review and Launch**, and then choose **Launch**\. When your new node starts, it applies the configurations specified by the recipes you have specified in the `RUN_LIST` parameter\.

1. Optional: If you have added the `nginx` cookbook to your run list, when you open the webpage linked to the public DNS of your new node, you should see a website that is hosted by your nginx web server\.

## Other Methods of Automating Repeated Runs of `chef-client`<a name="w4ab1b9c33c21"></a>

Although more difficult to achieve, and not recommended, you can run the script in this topic solely as part of standalone instance user data, use a AWS CloudFormation template to add it to new instance user data, configure a `cron` job to run the script regularly, or run `chef-client` within a service\. However, we recommend the Chef Client Cookbook method because there are some disadvantages with other automation techniques\.

For a complete list of parameters you can provide to `chef-client`, see the [Chef documentation](https://docs.chef.io/ctl_chef_client.html)\.

## Related Topics<a name="opscm-unattend-assoc-related"></a>

The following AWS blog posts offer more information about automatically associating nodes with your Chef Automate server, by using Auto Scaling groups, or within multiple accounts\.
+ [Using AWS OpsWorks for Chef Automate to Manage EC2 Instances with Auto Scaling](https://aws.amazon.com/blogs/mt/using-aws-opsworks-for-chef-automate-to-manage-ec2-instances-with-auto-scaling/)
+ [OpsWorks for Chef Automate – Automatically Bootstrapping Nodes in Different Accounts](https://aws.amazon.com/blogs/mt/opsworks-for-chef-automate-automatically-bootstrapping-nodes-in-different-accounts/)