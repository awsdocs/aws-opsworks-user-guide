# Adding Nodes Automatically in OpsWorks for Puppet Enterprise<a name="opspup-unattend-assoc"></a>

This topic describes how to add Amazon Elastic Compute Cloud \(Amazon EC2\) nodes to your OpsWorks for Puppet Enterprise server automatically\. In [Add Nodes for the Puppet Master to Manage](opspup-addnodes.md), you learned how to use the `associate-node` command to add one node at a time to your Puppet Enterprise server\. The code in this topic shows how to add nodes automatically using the unattended method\. The recommended method of unattended \(or automatic\) association of new nodes is to configure the Amazon EC2 user data\. By default, an OpsWorks for Puppet Enterprise server already has [https://puppet.com/docs/pe/2017.3/installing/installing_agents.html](https://puppet.com/docs/pe/2017.3/installing/installing_agents.html) available for for Ubuntu, Amazon Linux, and RHEL node operating systems\.

For information about how to disassociate a node, see [Disassociate a Node from an OpsWorks for Puppet Enterprise Server](opspup-disassociate-node.md) in this guide, and [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) in the OpsWorks for Puppet Enterprise API documentation\.

## Supported Operating Systems<a name="w4ab1b7c27b7"></a>

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

## Step 2: Create Instances by Using an Unattended Association Script<a name="w4ab1b7c27c11"></a>

To create EC2 instances, you can copy the following code to the `userdata` section of EC2 instance instructions, Amazon EC2 Auto Scaling group launch configurations, or an AWS CloudFormation template\. For more information about adding scripts to user data, see [Running Commands on Your Linux Instance at Launch](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) in the Amazon EC2 documentation\. The easiest way to create a new node is to use the [Amazon EC2 instance launch wizard](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)\. This walkthrough uses the Nginx web server example module setup described in [Getting Started with OpsWorks for Puppet Enterprise](gettingstarted-opspup.md)\.

1. This script runs the `opsworks-cm` API [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_AssociateNode.html) command to associate a new node with your Puppet master\. In this release, it also installs the current version of the AWS CLI on the node for you, in case it is not already running the most up\-to\-date version\. Save this script to a convenient location as `userdata.sh`\.

   By default, the name of the new registered node is the instance ID\.

   ```
   #!/bin/bash
   set -euo pipefail
   
   # put the OpsWorks name of your server if you don't use the ocm_server tag
   declare -x OCM_SERVER="serverName"
   # put the region of your OpsWorks configuration management server if you don't use the ocm_region tag
   declare -x OCM_REGION="region"
   
   # additional optional settings
   AWS_CLI_EXTRA_OPTS=(awsExtraOpts)
   CFN_SIGNAL="cfnSignal"
   
   # check operating system and install related packages
   function prepare_os_packages {
     local OS=`uname -a`
     if [[ ${OS} = *"Ubuntu"* ]]; then
       apt update && DEBIAN_FRONTEND=noninteractive apt -y upgrade
       apt -y install unzip python python-pip
       pip install awscli
     else
       yum -y update
       yum install -y git
     fi
   }
   
   function set_aws_settings {
     export PP_INSTANCE_ID=$(curl --silent --show-error --retry 3 http://169.254.169.254/latest/meta-data/instance-id)
     # this uses the EC2 instance ID as the node name
     export PP_IMAGE_NAME=$(curl --silent --show-error --retry 3 http://169.254.169.254/latest/meta-data/ami-id)
     export PP_REGION=$(curl --silent --show-error --retry 3 http://169.254.169.254/latest/meta-data/placement/availability-zone | sed 's/.$//')
   
   
     # Detecting if a tag is set. If so, override anything in the file
     export TAG_SERVER=$(aws ec2 describe-tags --region ${PP_REGION} --filters "Name=resource-id,Values=${PP_INSTANCE_ID}" \
     --query 'Tags[?Key==`ocm_server`].Value' --output text)
     export TAG_REGION=$(aws ec2 describe-tags --region ${PP_REGION} --filters "Name=resource-id,Values=${PP_INSTANCE_ID}" \
     --query 'Tags[?Key==`ocm_region`].Value' --output text)
   
     if [ -n ${TAG_SERVER} ] && [ ! -z ${TAG_SERVER} ]; then
       export OCM_SERVER=${TAG_SERVER}
     fi
   
     if [ -n ${TAG_REGION} ] && [ ! -z ${TAG_REGION} ]; then
       export OCM_REGION=${TAG_REGION}
     fi
   
     # set global settings
     export PUPPETSERVER=$(aws  opsworks-cm describe-servers --region=${OCM_REGION} ${AWS_CLI_EXTRA_OPTS[@]:-} \
     --query "Servers[?ServerName=='${OCM_SERVER}'].Endpoint" --output text)
     export PRUBY='/opt/puppetlabs/puppet/bin/ruby'
     export PUPPET='/opt/puppetlabs/bin/puppet'
     export DAEMONSPLAY='true'
     export SPLAYLIMIT='180'
     export PUPPET_CA_PATH='/etc/puppetlabs/puppet/ssl/certs/ca.pem'
   }
   
   function prepare_puppet {
     mkdir -p /opt/puppetlabs/puppet/cache/state
     mkdir -p /etc/puppetlabs/puppet/ssl/certs/
     mkdir -p /etc/puppetlabs/code/modules/
   
     echo "{\"disabled_message\":\"Locked by OpsWorks Deploy - $(date --iso-8601=seconds)\"}" > /opt/puppetlabs/puppet/cache/state/agent_disabled.lock
   }
   
   function establish_trust {
    aws opsworks-cm describe-servers --region=${OCM_REGION} --server-name ${OCM_SERVER} ${AWS_CLI_EXTRA_OPTS[@]:-} \
    --query "Servers[0].EngineAttributes[?Name=='PUPPET_API_CA_CERT'].Value" --output text > /etc/puppetlabs/puppet/ssl/certs/ca.pem
   }
   
   function install_puppet {
     ADD_EXTENSIONS=$(generate_csr_attributes)
     curl --cacert /etc/puppetlabs/puppet/ssl/certs/ca.pem --retry 3 "https://${PUPPETSERVER}:8140/packages/current/install.bash" | \
     /bin/bash -s agent:certname=${PP_INSTANCE_ID} \
     agent:splay=${DAEMONSPLAY} \
     extension_requests:pp_instance_id=${PP_INSTANCE_ID} \
     extension_requests:pp_region=${PP_REGION} \
     extension_requests:pp_image_name=${PP_IMAGE_NAME} ${ADD_EXTENSIONS}
   
     ${PUPPET} resource service puppet ensure=stopped
   }
   
   function generate_csr_attributes {
     pp_tags=$(aws ec2 describe-tags --region ${PP_REGION} --filters "Name=resource-id,Values=${PP_INSTANCE_ID}" \
     --query 'Tags[?starts_with(Key, `pp_`)].[Key,Value]' --output text | sed s/[[:blank:]]/=/)
     if [ -z ${pp_tags} ]; then
       # couldn't describe tags, check permissions in the IAM role
       return 0
     fi
   
     csr_attrs=""
     for i in $pp_tags; do
       csr_attrs="${csr_attrs} extension_requests:${i}"
     done
   
     echo ${csr_attrs}
   }
   
   function install_puppet_bootstrap {
     ${PUPPET} help bootstrap > /dev/null && bootstrap_installed=true || bootstrap_installed=false
     if [ "${bootstrap_installed}" = false ]; then
       echo "Puppet Bootstrap not present, installing"
       curl --retry 3 https://s3-eu-west-1.amazonaws.com/opsworks-cm-eu-west-1-beta-default-assets/misc/owpe/puppet-agent-bootstrap-0.2.1.tar.gz \
       -o /tmp/puppet-agent-bootstrap-0.2.1.tar.gz
       ${PUPPET} module install /tmp/puppet-agent-bootstrap-0.2.1.tar.gz --ignore-dependencies
       echo "Puppet Bootstrap installed"
     else
       echo "Puppet Bootstrap already present"
     fi
   }
   
   function run_puppet {
     sleep $[ ( ${RANDOM} % ${SPLAYLIMIT} ) + 1]s
     ${PUPPET} agent --enable
     ${PUPPET} agent --onetime --verbose --no-daemonize --no-usecacheonfailure --no-splay --show_diff
     ${PUPPET} resource service puppet ensure=running enable=true
   }
   
   function associate_node {
     CERTNAME=$(${PUPPET} config print certname --section agent)
     SSLDIR=$(${PUPPET} config print ssldir --section agent)
     PP_CSR_PATH="${SSLDIR}/certificate_requests/${CERTNAME}.pem"
     PP_CERT_PATH="${SSLDIR}/certs/${CERTNAME}.pem"
   
     # clear out extraneous certs and generate a new one
     ${PUPPET} bootstrap purge
     ${PUPPET} bootstrap csr
   
     # submit the cert
     ASSOCIATE_TOKEN=$(aws opsworks-cm associate-node --region ${OCM_REGION} --server-name ${OCM_SERVER} ${AWS_CLI_EXTRA_OPTS[@]:-} \
     --node-name ${CERTNAME} --engine-attributes Name=PUPPET_NODE_CSR,Value="`cat ${PP_CSR_PATH}`" --query "NodeAssociationStatusToken" --output text)
   
     # wait
     aws opsworks-cm wait node-associated --region ${OCM_REGION} --node-association-status-token "${ASSOCIATE_TOKEN}" \
     --server-name ${OCM_SERVER} ${AWS_CLI_EXTRA_OPTS[@]:-}
     # install and verify
     aws opsworks-cm describe-node-association-status --region ${OCM_REGION} --node-association-status-token "${ASSOCIATE_TOKEN}" \
     --server-name ${OCM_SERVER} ${AWS_CLI_EXTRA_OPTS[@]:-} --query 'EngineAttributes[0].Value' --output text > ${PP_CERT_PATH}
   
     ${PUPPET} bootstrap verify
   }
   
   # order of execution of functions
   prepare_os_packages
   set_aws_settings
   prepare_puppet
   establish_trust
   install_puppet
   install_puppet_bootstrap
   associate_node
   run_puppet
   
   touch /tmp/userdata.done
   eval ${CFN_SIGNAL}
   ```

1. Follow the procedure in [Launching an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the EC2 documentation, with modifications here\. In the EC2 instance launch wizard, choose an Amazon Linux AMI\.

1. On the **Configure Instance Details** page, select **myPuppetinstanceprofile**, the role you created in [Step 1: Create an IAM Role to Use as Your Instance Profile](#opspup-create-instance-profile), as your IAM role\.

1. In the **Advanced Details** area, upload the `userdata.sh` script that you created in Step 1\.

1. No changes are needed on the **Add Storage** page\. Go on to **Add Tags**\.

   By applying tags to your EC2 instance, you can customize the behavior of `userdata.sh`\. For this example, apply the role `nginx_webserver` to your node by adding the following tag: **pp\_role**, with the value **nginx\_webserver**\.

   Setting the `pp_role` value on the node sets data values that are permanently stored in the node's agent certificate, enabling trusted classification of the node\. For more information, see [Extension requests \(permanent certificate data\)](https://puppet.com/docs/puppet/5.1/ssl_attributes_extensions.html#extension-requests-permanent-certificate-data)) in the Puppet platform documentation\.

1. On the **Configure Security Group** page, choose **Add Rule**, and then choose the type **HTTP** to open port 80 for the Nginx web server in this example\.

1. Choose **Review and Launch**, and then choose **Launch**\. When your new node starts, it applies the Nginx configuration of the sample module you set up in [Getting Started with OpsWorks for Puppet Enterprise](gettingstarted-opspup.md)\.

1. When you open the webpage linked to the public DNS of your new node, you should see a website that is hosted by your Puppet\-managed Nginx web server\.