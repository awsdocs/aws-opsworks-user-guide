# Add Nodes for the Chef Server to Manage<a name="opscm-addnodes"></a>

The [https://docs.chef.io/chef_client.html](https://docs.chef.io/chef_client.html) agent runs Chef recipes on physical or virtual computers, called *nodes*, that are associated with the server\. You can connect on\-premises computers or instances to the Chef server to manage, provided the nodes are running supported operating systems\. Registering nodes with the Chef server installs the `chef-client` agent software on those nodes\.

You can use the following methods to add nodes:
+ Add notes individually by running a `knife` command that adds, or *bootstraps*, an EC2 instance so that the Chef server can manage it\. For more information see [Add nodes individually](opscm-addnodes-individually.md)\.
+ Add nodes automatically by using a script to perform unattended association of nodes with the Chef server\. The code in the [Starter Kit](opscm-starterkit.md) shows how to add nodes automatically using the unattended method\. For more information see, [Add nodes automatically in AWS OpsWorks for Chef Automate](opscm-unattend-assoc.md)\.

**Topics**
+ [Add nodes individually](opscm-addnodes-individually.md)
+ [Add nodes automatically in AWS OpsWorks for Chef Automate](opscm-unattend-assoc.md)