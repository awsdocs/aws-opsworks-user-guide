# Disassociate a Node from an OpsWorks for Puppet Enterprise Server<a name="opspup-disassociate-node"></a>

This section describes how to disassociate, or remove, a managed node from management by an OpsWorks for Puppet Enterprise server\. This operation is performed on the command line or in the Puppet Enterprise console; you cannot disassociate nodes in the OpsWorks for Puppet Enterprise management console\. Currently, the OpsWorks for Puppet Enterprise API does not allow for batch removal of multiple nodes\. The command in this section disassociates one node at a time\.

We recommend that you disassociate nodes from a Puppet master before you delete the server, so that the nodes continue to operate without trying to reconnect with the server\. To do this, run the [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) AWS CLI command\. To completely remove a node from PE, you must disassociate the node and revoke its certificate, so that the node does not continuously attempt to check in with the Puppet master\. You should also [uninstall `puppet-agent` from nodes](https://puppet.com/docs/pe/2017.3/installing/uninstalling.html#uninstall-agents) when you no longer want to manage them by using the Puppet master\.

**To disassociate nodes**

1. In the AWS CLI, run the following command to disassociate nodes\. *Node\_name* is the name of the node that you want to disassociate; for Amazon EC2 instances, this is the instance ID\.*Server\_name* is the name of the Puppet master from which you want to disassociate the node\. Both parameters are required\. The `--region` parameter is not required unless you want to disassociate a node from a Puppet master that is not in your default region\.

   ```
   aws opsworks-cm --region Region_name disassociate-node --node-name Node_name --server-name Server_name
   ```

   The following command is an example\.

   ```
   aws opsworks-cm --region us-west-2 disassociate-node --node-name i-0010zzz00d66zzz90 --server-name opsworkstest
   ```

1. Wait until a response message indicates that the disassociation is finished\.

For more information about how to delete an OpsWorks for Puppet Enterprise server, see [Delete an OpsWorks for Puppet Enterprise Server](opspup-delete-server.md)\.

## See Also<a name="w4ab1b7c29c11"></a>
+ [Remove nodes](https://puppet.com/docs/pe/2017.3/managing_nodes/adding_and_removing_nodes.html#remove-nodes) in the Puppet Enterprise documentation