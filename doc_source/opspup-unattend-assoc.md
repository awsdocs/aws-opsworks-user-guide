# Adding Nodes Automatically in OpsWorks for Puppet Enterprise<a name="opspup-unattend-assoc"></a>

This topic describes how to add Amazon Elastic Compute Cloud \(Amazon EC2\) nodes to your OpsWorks for Puppet Enterprise server automatically\. In [Add Nodes for the Puppet Master to Manage](opspup-addnodes.md), you learned how to use the `associate-node` command to add one node at a time to your Puppet Enterprise server\. The code in this topic shows how to add nodes automatically using the unattended method\. The recommended method of unattended \(or automatic\) association of new nodes is to configure the Amazon EC2 user data\. By default, an OpsWorks for Puppet Enterprise server already has [https://puppet.com/docs/pe/2019.8/installing_agents.html](https://puppet.com/docs/pe/2019.8/installing_agents.html) available for for Ubuntu, Amazon Linux, and RHEL node operating systems\.

For information about how to disassociate a node, see [Disassociate a Node from an OpsWorks for Puppet Enterprise Server](opspup-disassociate-node.md) in this guide, and [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) in the OpsWorks for Puppet Enterprise API documentation\.

## Supported Operating Systems<a name="opspup-unattend-os"></a>

For the current list of supported operating systems for nodes, see the [Puppet agent platforms](https://docs.puppet.com/pe/latest/sys_req_os.html#puppet-agent-platforms)\.

## Step 1: Create an IAM Role to Use as Your Instance Profile<a name="opspup-create-instance-profile"></a>

Create an AWS Identity and Access Management \(IAM\) role to use as your EC2 instance profile, and attach the following policy to the IAM role\. This policy allows the `opsworks-cm` API to communicate with the EC2 instance during node registration\. For more information about instance profiles, see [Using Instance Profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the Amazon EC2 documentation\. For information about how to create an IAM role, see [Creating an IAM Role in the Console](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#create-iam-role-console) in the Amazon EC2 documentation\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "opsworks-cm:AssociateNode",
                "opsworks-cm:DescribeNodeAssociationStatus",
                "opsworks-cm:DescribeServers",
                "ec2:DescribeTags"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

AWS OpsWorks provides an AWS CloudFormation template that you can use to create the IAM role with the preceding policy statement\. The following AWS CLI command creates the instance profile role for you by using this template\. You can omit the `--region` parameter if you want to create the new AWS CloudFormation stack in your default region\.

```
aws cloudformation --region region ID create-stack --stack-name myPuppetinstanceprofile --template-url https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/owpe/opsworks-cm-nodes-roles.yaml --capabilities CAPABILITY_IAM
```

## Step 2: Create Instances by Using an Unattended Association Script<a name="opspup-unattend-script"></a>

To create EC2 instances, you can copy the user data script that is included in the [Starter Kit](opspup-starterkit.md) to the `userdata` section of EC2 instance instructions, Amazon EC2 Auto Scaling group launch configurations, or an AWS CloudFormation template\. The script is supported only for EC2 instances running Ubuntu and Amazon Linux operating systems\. For more information about adding scripts to user data, see [Running Commands on Your Linux Instance at Launch](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) in the Amazon EC2 documentation\. The easiest way to create a new node is to use the [Amazon EC2 instance launch wizard](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)\. This walkthrough uses the Apache web server example module setup described in [Getting Started with OpsWorks for Puppet Enterprise](gettingstarted-opspup.md)\.

1. The user data script in the Starter Kit runs the `opsworks-cm` API [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html) command to associate a new node with your Puppet master\. In this release, it also installs the current version of the AWS CLI on the node for you, in case it is not already running the most up\-to\-date version\. Save this script to a convenient location as `userdata.sh`\.

   By default, the name of the new registered node is the instance ID\.

1. Follow the procedure in [Launching an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the EC2 documentation, with modifications here\. In the EC2 instance launch wizard, choose an Amazon Linux AMI\.

1. On the **Configure Instance Details** page, select **myPuppetinstanceprofile**, the role you created in [Step 1: Create an IAM Role to Use as Your Instance Profile](#opspup-create-instance-profile), as your IAM role\.

1. In the **Advanced Details** area, upload the `userdata.sh` script that you created in Step 1\.

1. No changes are needed on the **Add Storage** page\. Go on to **Add Tags**\.

   By applying tags to your EC2 instance, you can customize the behavior of `userdata.sh`\. For this example, apply the role `apache_webserver` to your node by adding the following tag: **pp\_role**, with the value **apache\_webserver**\.

   Setting the `pp_role` value on the node sets data values that are permanently stored in the node's agent certificate, enabling trusted classification of the node\. For more information, see [Extension requests \(permanent certificate data\)](https://puppet.com/docs/puppet/5.1/ssl_attributes_extensions.html#extension-requests-permanent-certificate-data)) in the Puppet platform documentation\.

1. On the **Configure Security Group** page, choose **Add Rule**, and then choose the type **HTTP** to open port 8080 for the Apache web server in this example\.

1. Choose **Review and Launch**, and then choose **Launch**\. When your new node starts, it applies the Apache configuration of the sample module you set up in [Set Up the Starter Kit Apache Example](opspup-starterkit.md#opspup-post-launch-web-server)\.

1. When you open the webpage linked to the public DNS of your new node, you should see a website that is hosted by your Puppet\-managed Apache web server\.