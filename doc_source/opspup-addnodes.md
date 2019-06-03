# Add Nodes for the Puppet Master to Manage<a name="opspup-addnodes"></a>

**Topics**
+ [Supported Operating Systems](#w4ab1b7c19c13c13)
+ [Run `associateNode()` API calls](#w4ab1b7c19c13c15)
+ [Considerations for Adding On\-premises Nodes](#w4ab1b7c19c13c17)
+ [More Information](#w4ab1b7c19c13c19)

The recommended way to add nodes is by using the AWS OpsWorks `associateNode()` API\. The Puppet Enterprise master server hosts a repository that you use to install the Puppet agent software on nodes that you want to manage, whether nodes are on\-premises physical computers or virtual machines\. Puppet agent software for some operating systems is installed on the OpsWorks for Puppet Enterprise server as part of the launch process\. The following table shows the operating system agents that are available on your OpsWorks for Puppet Enterprise server at launch\.


**Preinstalled operating system agents**  

| Supported operating system | Versions | 
| --- | --- | 
| Ubuntu | 18\.04, 14\.04, 16\.04 | 
| Red Hat Enterprise Linux \(RHEL\) | 6 | 
| Windows | 64\-bit editions of all [Puppet\-supported](https://puppet.com/docs/pe/2017.3/installing/supported_operating_systems.html#agent-platforms) Windows releases | 

You can add `puppet-agent` to your server for other operating systems\. Be aware that system maintenance will delete agents that you have added to your server after launch\. Although most existing attached nodes that are already running the deleted agent continue to check in, nodes running Debian operating systems can stop reporting\. We recommend that you manually install `puppet-agent` on nodes that are running operating systems for which the agent software is not preinstalled on your OpsWorks for Puppet Enterprise server\. For detailed information about how to make `puppet-agent` available on your server for nodes with other operating systems, see [Installing agents](https://puppet.com/docs/pe/2017.3/installing/installing_agents.html) in the Puppet Enterprise documentation\.

For information about how to associate nodes with your Puppet master automatically by populating EC2 instance user data, see [Adding Nodes Automatically in OpsWorks for Puppet Enterprise](opspup-unattend-assoc.md)\.

## Supported Operating Systems<a name="w4ab1b7c19c13c13"></a>

For the current list of supported operating systems for nodes, see the [Puppet agent platforms](https://docs.puppet.com/pe/latest/sys_req_os.html#puppet-agent-platforms) in the Puppet Enterprise documentation\.

## Run `associateNode()` API calls<a name="w4ab1b7c19c13c15"></a>

After you add nodes by installing `puppet-agent`, nodes send certificate signing requests \(CSRs\) to the OpsWorks for Puppet Enterprise server\. You can view the CSRs in the Puppet console; for more information about node CSRs, see [Managing certificate signing requests](https://puppet.com/docs/pe/2017.3/managing_nodes/adding_and_removing_nodes.html#managing-certificate-signing-requests) in the Puppet Enterprise documentation\. Running the OpsWorks for Puppet Enterprise `associateNode()` API call processes node CSRs, and associates the node with your server\. The following is an example of how to use this API call in the AWS CLI to associate a single node\. You will need the PEM\-formatted CSR that the node sends; you can get this from the Puppet console\.

```
aws opsworks-cm associate-node --server-name "test-puppet-server" --node-name "node or instance ID" --engine-attributes "Name=PUPPET_NODE_CSR,Value='PEM_formatted_CSR_from_the_node'
```

For more information about how to add nodes automatically by using `associateNode()`, see [Adding Nodes Automatically in OpsWorks for Puppet Enterprise](opspup-unattend-assoc.md)\.

## Considerations for Adding On\-premises Nodes<a name="w4ab1b7c19c13c17"></a>

After you have installed `puppet-agent` on your on\-premises computers or virtual machines, you can use either of two ways to associate on\-premises nodes with your OpsWorks for Puppet Enterprise master\.
+ If a node supports installation of the [AWS SDK](https://aws.amazon.com/tools/), [AWS CLI](https://aws.amazon.com/cli/), or [AWS Tools for PowerShell](https://aws.amazon.com/powershell/), you can use the recommended method for associating a node, which is to run an `associateNode()` API call\. The starter kit that you download when you first create an OpsWorks for Puppet Enterprise master shows how to assign roles to nodes by using tags\. You can apply tags at the same time that you are associating nodes with the Puppet master by specifying trusted facts in the CSR\. For example, the demo control repository that is included with the starter kit is configured to use the tag `pp_role` to assign roles to Amazon EC2 instances\. For more information about how to add tags to a CSR as trusted facts, see [Extension requests \(permanent certificate data\)](https://puppet.com/docs/puppet/5.1/ssl_attributes_extensions.html#extension-requests-permanent-certificate-data)) in the Puppet platform documentation\.
+ If the node cannot run AWS management or development tools, you can still register it with your OpsWorks for Puppet Enterprise master the same way you would register it with any unmanaged Puppet Enterprise master\. As mentioned in this topic, installing `puppet-agent` sends a CSR to the OpsWorks for Puppet Enterprise master\. An authorized Puppet user can sign the CSR manually, or configure automatic signing of CSRs by editing the `autosign.conf` file that is stored on the Puppet master\. For more information about configuring autosigning and editing `autosign.conf`, see [SSL configuration: autosigning certificate requests](https://puppet.com/docs/puppet/5.3/ssl_autosign.html) in the Puppet platform documentation\.

To associate on\-premises nodes with a Puppet master and allow the Puppet master to accept all CSRs, do the following in the Puppet Enterprise console\. The parameter that controls the behavior is `puppet_enterprise::profile::master::allow_unauthenticated_ca`\.

**Important**  
Enabling the Puppet master to accept self\-signed CSRs or all CSRs is not recommended for security reasons\. By default, allowing unauthenticated CSRs makes a Puppet master accessible to the world\. Setting the upload of certificate requests to be enabled by default can make your Puppet master vulnerable to denial of service \(DoS\) attacks\.

1. Sign in to the Puppet Enterprise console\.

1. Choose **Configure**, choose **Classification**, choose **PE Master**, and then choose the **Configuration** tab\.

1. On the **Classification** tab, locate the class **puppet\_enterprise::profile::master**\.

1. Set the value of the **allow\_unauthenticated\_ca** parameter to **true**\.

1. Save your changes\. Your changes are applied during the next Puppet run\. You can allow 30 minutes for changes to take effect \(and on\-premises nodes to be added\), or you can initiate a Puppet run manually in the **Run** section of the PE console\.

## More Information<a name="w4ab1b7c19c13c19"></a>

Visit the [Learn Puppet tutorial site](https://learn.puppet.com/) to learn more about using OpsWorks for Puppet Enterprise servers and Puppet Enterprise console features\.