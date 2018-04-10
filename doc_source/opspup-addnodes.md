# Add Nodes for the Puppet Master to Manage<a name="opspup-addnodes"></a>


+ [Supported Operating Systems](#w3ab2b7c15c13c13)
+ [Run `associateNode()` API calls](#w3ab2b7c15c13c15)
+ [More Information](#w3ab2b7c15c13c17)

The Puppet Enterprise master server hosts a repository that you use to install the Puppet agent software on nodes that you want to manage\. Puppet agent software for some operating systems is installed on the AWS OpsWorks for Puppet Enterprise server as part of the launch process\. The following table shows the operating system agents that are available on your AWS OpsWorks for Puppet Enterprise server at launch\.


**Preinstalled operating system agents**  

| Supported operating system | Versions | 
| --- | --- | 
| Ubuntu | 14\.04, 16\.04 | 
| Red Hat Enterprise Linux \(RHEL\) | 6 | 
| Windows | 64\-bit editions of all [Puppet\-supported](https://puppet.com/docs/pe/2017.3/installing/supported_operating_systems.html#agent-platforms) Windows releases | 

You can add `puppet-agent` to your server for other operating systems\. Be aware that system maintenance will delete agents that you have added to your server after launch\. Although most existing attached nodes that are already running the deleted agent continue to check in, nodes running Debian operating systems can stop reporting\. We recommend that you manually install `puppet-agent` on nodes that are running operating systems for which the agent software is not preinstalled on your AWS OpsWorks for Puppet Enterprise server\. For detailed information about how to make `puppet-agent` available on your server for nodes with other operating systems, see [Installing agents](https://puppet.com/docs/pe/2017.3/installing/installing_agents.html) in the Puppet Enterprise documentation\.

For information about how to associate nodes with your Puppet master automatically by populating EC2 instance user data, see \.

## Supported Operating Systems<a name="w3ab2b7c15c13c13"></a>

For the current list of supported operating systems for nodes, see the [Puppet agent platforms](https://docs.puppet.com/pe/latest/sys_req_os.html#puppet-agent-platforms) in the Puppet Enterprise documentation\.

## Run `associateNode()` API calls<a name="w3ab2b7c15c13c15"></a>

After you add nodes by installing `puppet-agent`, nodes send certificate signing requests \(CSRs\) to the AWS OpsWorks for Puppet Enterprise server\. You can view the CSRs in the Puppet console; for more information about node CSRs, see [Managing certificate signing requests](https://puppet.com/docs/pe/2017.3/managing_nodes/adding_and_removing_nodes.html#managing-certificate-signing-requests) in the Puppet Enterprise documentation\. Running the AWS OpsWorks for Puppet Enterprise `associateNode()` API call processes node CSRs, and associates the node with your server\. The following is an example of how to use this API call in the AWS CLI to associate a single node\. You will need the PEM\-formatted CSR that the node sends; you can get this from the Puppet console\.

```
aws opsworks-cm associate-node --server-name "test-puppet-server" --node-name "node or instance ID" --engine-attributes "Name=PUPPET_NODE_CSR,Value='PEM_formatted_CSR_from_the_node'
```

For more information about how to add nodes automatically by using `associateNode()`, see \.

## Associating On-Premise Infrastructure

After `puppet-agent` has been installed on your on-premise servers or virtual machines, there are two approaches available for associating them with your Puppet Enterprise server\.

If the node in question supports installation of the [AWS SDK](https://aws.amazon.com/tools/), [AWS Command Line Interface](https://aws.amazon.com/cli/), or [AWS Tools for Windows Powershell](https://aws.amazon.com/powershell/), the `associateNode()` API call can be used. This provides a seamless experience for managing nodes both on-premise and in the cloud. As shown in the starter kit provided when you first create an OpsWorks for Puppet Enterprise server, it is possible to assign roles to nodes using tagging. This same strategy can be applied to on-premise infrastructure by providing trusted facts in the CSR. For example, the demo control repository is configured to use the tag `pp_role` to assign roles to Amazon EC2 instances.

If the node in question does not support one of the above tools, you can still register it with your OpsWorks for Puppet Enterprise server the same way as any unmanaged Puppet Enterprise master. As mentioned above, installation of `puppet-agent` generates a CSR to the OpsWorks for Puppet Enterprise server. This can then be manually signed by an authorized Puppet user, or automatically signed by updating the needed configuration in `autosign.conf` on the OpsWorks for Puppet Enterprise server. For more information on configuring autosigning, see [SSL configuration: autosigning certificate requests](https://puppet.com/docs/puppet/5.3/ssl_autosign.html).

## More Information<a name="w3ab2b7c15c13c17"></a>

Visit the [Learn Puppet tutorial site](https://learn.puppet.com/) to learn more about using AWS OpsWorks for Puppet Enterprise servers and Puppet Enterprise console features\.
