# Delete an AWS OpsWorks for Chef Automate Server<a name="opscm-delete-server"></a>

This section describes how to delete an AWS OpsWorks for Chef Automate server\. Deleting a server also deletes its events, logs, and any cookbooks that were stored on the server\. Supporting resources \(Amazon Elastic Compute Cloud instance, Amazon Elastic Block Store volume, etc\.\) are deleted also, along with all automated backups\.

Although deleting a server does not delete nodes, they are no longer managed by the deleted server, and will continuously attempt to reconnect\. For this reason, we recommend disassociating managed nodes before you delete a Chef server\. In this release, you can disassociate nodes by running an AWS CLI command\.

## Step 1: Disassociate Managed Nodes<a name="w4ab1b9c37b7"></a>

Disassociate nodes from the Chef server before you delete the server, so that the nodes continue to operate without trying to reconnect with the server\. To do this, run the [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) AWS CLI command\.

**To disassociate nodes**

1. In the AWS CLI, run the following command to disassociate nodes\. *Server\_name* is the name of the Chef server from which you want to disassociate the node\.

   ```
   aws opsworks-cm --region Region_name disassociate-node --node-name Node_name --server-name Server_name
   ```

1. Wait until a response message indicates that the disassociation is finished\.

## Step 2: Delete the Server<a name="w4ab1b9c37b9"></a>

1. On the server’s tile on the dashboard, expand the** Actions** menu\.

1. Choose **Delete server**\.

1. When you are prompted to confirm the deletion, choose **Yes**\.