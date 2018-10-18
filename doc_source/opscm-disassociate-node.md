# Disassociate a Node from an AWS OpsWorks for Chef Automate Server<a name="opscm-disassociate-node"></a>

This section describes how to disassociate, or remove, a managed node from management by an AWS OpsWorks for Chef Automate server\. This operation is performed on the command line; you cannot disassociate nodes in the AWS OpsWorks for Chef Automate management console\. Currently, the AWS OpsWorks for Chef Automate API does not allow for batch removal of multiple nodes\. The command in this section disassociates one node at a time\.

We recommend that you disassociate nodes from a Chef server before you delete the server, so that the nodes continue to operate without trying to reconnect with the server\. To do this, run the [http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/API_DisassociateNode.html) AWS CLI command\.

**To disassociate nodes**

1. In the AWS CLI, run the following command to disassociate nodes\. *Node\_name* is the name of the node that you want to disassociate; for Amazon EC2 instances, this is the instance ID\. *Server\_name* is the name of the Chef server from which you want to disassociate the node\. `--engine-attributes` specifies your default `CHEF_ORGANIZATION` name\. All three of these parameters are required\.

   The `--region` parameter is not required unless you want to disassociate a node from a Chef server that is not in your default region\.

   ```
   aws opsworks-cm --region Region_name disassociate-node --node-name Node_name --server-name Server_name --engine-attributes "Name=CHEF_ORGANIZATION,Value='default'"
   ```

   The following command is an example\.

   ```
   aws opsworks-cm --region us-west-2 disassociate-node --node-name i-0010zzz00d66zzz90 --server-name opsworkstest --engine-attributes "Name=CHEF_ORGANIZATION,Value='default'"
   ```

1. Wait until a response message indicates that the disassociation is finished\.

   After you successfully disassociate a node from an AWS OpsWorks for Chef Automate server, it might still be visible in the Chef Automate dashboard\. By default, Chef enforces a retention period for node state information, and purges the node automatically after a few days\. For more information about data retention management and the Chef Reaper tool, see [Data Retention Management](https://docs.chef.io/data_retention_chef_automate.html) in the Chef documentation\.

For more information about how to delete an AWS OpsWorks for Chef Automate server, see [Delete an AWS OpsWorks for Chef Automate Server](opscm-delete-server.md)\.

## Related Topics<a name="opscm-disassoc-related"></a>

The following AWS blog posts offer more information about automatically associating nodes with your Chef Automate server, by using Auto Scaling groups, or within multiple accounts\.
+ [Using AWS OpsWorks for Chef Automate to Manage EC2 Instances with Auto Scaling](https://aws.amazon.com/blogs/mt/using-aws-opsworks-for-chef-automate-to-manage-ec2-instances-with-auto-scaling/)
+ [OpsWorks for Chef Automate – Automatically Bootstrapping Nodes in Different Accounts](https://aws.amazon.com/blogs/mt/opsworks-for-chef-automate-automatically-bootstrapping-nodes-in-different-accounts/)