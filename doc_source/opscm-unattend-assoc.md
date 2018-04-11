# Adding Nodes Automatically in AWS OpsWorks for Chef Automate<a name="opscm-unattend-assoc"></a>

This topic describes how to add Amazon Elastic Compute Cloud \(Amazon EC2\) nodes to your Chef server automatically\. In , you learned how to use the [https://docs.chef.io/install_bootstrap.html#knife-bootstrap](https://docs.chef.io/install_bootstrap.html#knife-bootstrap) command to add one node at a time to your Chef server\. The code in this topic shows how to add nodes automatically using the unattended method\. The recommended method of unattended \(or automatic\) association of new nodes is to configure the [Chef Client Cookbook](https://supermarket.chef.io/cookbooks/chef-client)\. Before you run the `chef-client` agent, upload the Chef Client cookbook to your Chef server, and then install the `chef-client` agent in service mode with, for example, an HTTPD role, as shown in the following sample command\.

```
chef-client -r "chef-client,role[httpd]"
```

To communicate with the Chef server, the `chef-client` agent software must have access to the public key of the client node\. You can generate a public\-private key pair in Amazon EC2, and then pass the public key to the AWS OpsWorks `associate-node` API call with the node name\. The script shown in this topic, included in the Starter Kit, gathers your organization name, server name, and server endpoint for you\. This ensures that the node is associated with the Chef server, and the `chef-client` agent software that runs on the node can communicate with the server after matching the private key\.

The minimum supported version of `chef-client` on nodes associated with an AWS OpsWorks for Chef Automate server is 13\.*x*\. We recommend running the [most current, stable `chef-client` version](https://downloads.chef.io/chef/stable)\.

For information about how to disassociate a node, see  in this guide, and [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) in the AWS OpsWorks for Chef Automate API documentation\.

## Supported Operating Systems<a name="w3ab2b9c27c13"></a>

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
                "opsworks-cm:DescribeNodeAssociationStatus"
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

## Step 2: Install the Chef Client Cookbook<a name="w3ab2b9c27c17"></a>

If you have not done so already, follow the steps in  to ensure that your Berksfile references the Chef Client cookbook and installs the cookbook\.

## Step 3: Create Instances by Using an Unattended Association Script<a name="opscm-unattend-script"></a>

1. To create EC2 instances, you can copy the following code to the `userdata` section of EC2 instance instructions, Amazon EC2 Auto Scaling group launch configurations, or an AWS CloudFormation template\. You do not need to prefill your own values in this script\. For more information about adding scripts to user data, see [Running Commands on Your Linux Instance at Launch](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) in the Amazon EC2 documentation\.

   This script runs the `opsworks-cm` API [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html) command to associate a new node with your Chef server\. In this release, it also installs the current version of the AWS CLI on the node for you, in case it is not already running the most up\-to\-date version\.

   By default, the name of the new registered node is the instance ID, but you can change the name by modifying the value of the `NODE_NAME` variable\. Because changing the organization name in the Chef console UI is currently not possible, leave `CHEF_ORGANIZATION` set to `default`\.

   ```
   #!/bin/bash
   
   # Required settings
   NODE_NAME="$(curl --silent --show-error --retry 3 http://169.254.169.254/latest/meta-data/instance-id)" # This uses the EC2 instance ID as the node name
   REGION="region" # Valid values are us-east-1, us-west-2, or eu-west-1
   CHEF_SERVER_NAME="serverName" # The name of your Chef server
   CHEF_SERVER_ENDPOINT="endpoint" # Provide the FQDN or endpoint; it's the string after 'https://'
   
   # Optional settings
   CHEF_ORGANIZATION="default"    # Leave as "default"; do not change. AWS OpsWorks for Chef Automate always creates the organization "default"
   NODE_ENVIRONMENT=""            # e.g. development, staging, onebox ...
   CHEF_CLIENT_VERSION="13.8.3" # latest if empty
   
   # Recommended: upload the chef-client cookbook from the chef supermarket  https://supermarket.chef.io/cookbooks/chef-client
   # Use this to apply sensible default settings for your chef-client configuration like logrotate, and running as a service.
   # You can add more cookbooks in the run list, based on your needs
   RUN_LIST="" # e.g. "recipe[chef-client],recipe[apache2]"
   
   # ---------------------------
   set -e -o pipefail
   
   AWS_CLI_TMP_FOLDER=$(mktemp --directory "/tmp/awscli_XXXX")
   CHEF_CA_PATH="/etc/chef/opsworks-cm-ca-2016-root.pem"
   
   install_aws_cli() {
     # see: http://docs.aws.amazon.com/cli/latest/userguide/installing.html#install-bundle-other-os
     cd "$AWS_CLI_TMP_FOLDER"
     curl --retry 3 -L -o "awscli-bundle.zip" "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip"
     unzip "awscli-bundle.zip"
     ./awscli-bundle/install -i "$PWD"
   }
   
   aws_cli() {
     "${AWS_CLI_TMP_FOLDER}/bin/aws" opsworks-cm --region "${REGION}" --output text "$@" --server-name "${CHEF_SERVER_NAME}"
   }
   
   associate_node() {
     client_key="/etc/chef/client.pem"
     mkdir /etc/chef
     ( umask 077; openssl genrsa -out "${client_key}" 2048 )
   
     aws_cli associate-node \
       --node-name "${NODE_NAME}" \
       --engine-attributes \
       "Name=CHEF_ORGANIZATION,Value=${CHEF_ORGANIZATION}" \
       "Name=CHEF_NODE_PUBLIC_KEY,Value='$(openssl rsa -in "${client_key}" -pubout)'"
   }
   
   write_chef_config() {
     (
       echo "chef_server_url   'https://${CHEF_SERVER_ENDPOINT}/organizations/${CHEF_ORGANIZATION}'"
       echo "node_name         '${NODE_NAME}'"
       echo "ssl_ca_file       '${CHEF_CA_PATH}'"
     ) >> /etc/chef/client.rb
   }
   
   install_chef_client() {
     # see: https://docs.chef.io/install_omnibus.html
     curl --silent --show-error --retry 3 --location https://omnitruck.chef.io/install.sh | bash -s -- -v "${CHEF_CLIENT_VERSION}"
   }
   
   install_trusted_certs() {
     curl --silent --show-error --retry 3 --location --output "${CHEF_CA_PATH}" \
       "https://opsworks-cm-${REGION}-prod-default-assets.s3.amazonaws.com/misc/opsworks-cm-ca-2016-root.pem"
   }
   
   wait_node_associated() {
     aws_cli wait node-associated --node-association-status-token "$1"
   }
   
   install_aws_cli
   node_association_status_token="$(associate_node)"
   install_chef_client
   write_chef_config
   install_trusted_certs
   wait_node_associated "${node_association_status_token}"
   
   if [ -z "${NODE_ENVIRONMENT}" ]; then
     chef-client -r "${RUN_LIST}"
   else
     chef-client -r "${RUN_LIST}" -E "${NODE_ENVIRONMENT}"
   fi
   ```

1. Follow the procedure in [Launching an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the EC2 documentation, with modifications here\. In the EC2 instance launch wizard, choose an Amazon Linux AMI\.

1. On the **Configure Instance Details** page, select the role you created in [Step 1: Create an IAM Role to Use as Your Instance Profile](#opscm-create-instance-profile), as your IAM role\.

1. In the **Advanced Details** area, upload the `userdata.sh` script that you created earlier in this procedure\.

1. No changes are needed on the **Add Storage** page\. Go on to **Add Tags**\.

   By applying tags to your EC2 instance, you can customize the behavior of `userdata.sh` to instances that have specific tags\. For this example, apply the role `apache-server` to your node by adding the following tag: **CA\_role**, with the value **apache\-server**\.

1. On the **Configure Security Group** page, choose **Add Rule**, and then choose the type **HTTP** to open port numbers 443 and 80 for the Apache web server in this example\.

1. Choose **Review and Launch**, and then choose **Launch**\. When your new node starts, it applies the configurations specified by the recipes you have specified in the `RUN_LIST` parameter\.

1. Optional: If you have added the `apache2` cookbook to your run list, when you open the webpage linked to the public DNS of your new node, you should see a website that is hosted by your Apache web server\.

## Other Methods of Automating Repeated Runs of `chef-client`<a name="w3ab2b9c27c21"></a>

Although more difficult to achieve, and not recommended, you can run the script in this topic solely as part of standalone instance user data, use a AWS CloudFormation template to add it to new instance user data, configure a `cron` job to run the script regularly, or run `chef-client` within a service\. However, we recommend the Chef Client Cookbook method because of the following disadvantages of other automation techniques:

+ The `chef-client` agent runs in the foreground of instance processes, and logs to `stdout`\. Because it is part of instance user data, the output is stored in `/var/log/cloud-init-output.log`\. This directory is not rotated—or purged—and eventually runs out of disk space\.

+ Automation of this script doesn't survive or resume automatically if the node is restarted, or the process is terminated in some other way \(for example, if the Chef server is restarted\)\.

+ Because `chef-client` runs processes in the foreground, its tasks in an instance's user data would never finish, and the instance could not signal other processes that it is finished booting\. On Ubuntu nodes, for example, this can prevent other services from starting, and automatic updates from running\.

Optionally, you can add one or more of the following parameters for `chef-client` in the last lines of the preceding script:

+ `--runlist RunlistItem1,RunlistItem2,...`

  Adds run list items to the initial Chef run list\.

+ `--environment ENVIRONMENT`

  Sets the Chef environment on the node\.

+ `--json-attributes JSON_ATTRIBS`

  Loads attributes from a JSON file or URL\.

For a complete list of parameters you can provide to `chef-client`, see the [Chef documentation](https://docs.chef.io/ctl_chef_client.html)\.

The user data shown in this topic describes how to install `chef-client`, and perform authentication similarly to `knife bootstrap`\. To run specific recipes on a new node, associate a role or environment with the node, and then pass the parameter to the `chef-client` command\.

## Related Topics<a name="opscm-unattend-assoc-related"></a>

The following AWS blog posts offer more information about automatically associating nodes with your Chef Automate server, by using Auto Scaling groups, or within multiple accounts\.

+ [Using AWS OpsWorks for Chef Automate to Manage EC2 Instances with Auto Scaling](https://aws.amazon.com/blogs/mt/using-aws-opsworks-for-chef-automate-to-manage-ec2-instances-with-auto-scaling/)

+ [OpsWorks for Chef Automate – Automatically Bootstrapping Nodes in Different Accounts](https://aws.amazon.com/blogs/mt/opsworks-for-chef-automate-automatically-bootstrapping-nodes-in-different-accounts/)