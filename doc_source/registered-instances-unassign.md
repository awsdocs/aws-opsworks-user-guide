# Unassigning a Registered Instance<a name="registered-instances-unassign"></a>

**Note**  
This feature is supported only for Linux stacks\.

To remove a registered instance from its layers, on the **Instances** page, choose the instance name and then choose **Unassign**\. AWS OpsWorks Stacks then runs the layer's Shutdown recipes on the instance\. These recipes perform tasks such as shutting down services but do not stop the instance\. If the instance is assigned to multiple layers, unassign applies to every layer; you can't unassign an instance from a subset of its layers\. However, the instance is still registered with the stack, and you can assign it to another layer if you wish\.