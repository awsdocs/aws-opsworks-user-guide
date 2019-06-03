# Delete an OpsWorks for Puppet Enterprise Server<a name="opspup-delete-server"></a>

This section describes how to delete an OpsWorks for Puppet Enterprise server\. Deleting a server also deletes its events, logs, and any modules that were stored on the server\. Supporting resources \(Amazon Elastic Compute Cloud instance, Amazon Elastic Block Store volume, etc\.\) are deleted also, along with all automated backups\.

Although deleting a server does not delete nodes, they are no longer managed by the deleted server, and will continuously attempt to reconnect\. For this reason, we recommend disassociating managed nodes before you delete a Puppet master\. In this release, you can disassociate nodes by running an AWS CLI command\.

## Step 1: Disassociate Managed Nodes<a name="w4ab1b7c31b7"></a>

Disassociate nodes from the Puppet master before you delete the server, so that the nodes continue to operate without trying to reconnect with the server\. To do this, run the [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) AWS CLI command\.

**To disassociate nodes**

1. In the AWS CLI, run the following command to disassociate nodes\. *Server\_name* is the name of the Puppet master from which you want to disassociate the node\. The value of `--node-name` can be an instance ID\.

   ```
   aws opsworks-cm --region Region_name disassociate-node --node-name Node_name --server-name Server_name
   ```

1. Wait until a response message indicates that the disassociation is finished\.

## Step 2: Delete the Server<a name="w4ab1b7c31b9"></a>

1. On the server’s tile on the dashboard, expand the** Actions** menu\.  
![\[OpsWorks for Puppet Enterprise Delete Server command\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/opspup_prop_delete.png)

1. Choose **Delete Puppet Enterprise server**\.

1. When you are prompted to confirm the deletion, fill in the check box to delete associated roles and resources, and then choose **Yes, Delete**\.

## See Also<a name="w4ab1b7c31c11"></a>
+ [Disassociate a Node from an OpsWorks for Puppet Enterprise Server](opspup-disassociate-node.md)