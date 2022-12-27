# Step 7: Start an instance<a name="migrating-to-systems-manager-start-instance"></a>

After you've provisioned an instance, you are ready to test the instance\. At this point, there are no instances running\.

To take your instances online, adjust the `Min`, `Max`, and `Desired capacity` values for the Auto Scaling group to a number that makes sense for your application\. Initially, you may want to set these values to 1, to bring a single instance online, and verify the instance performs all expected actions including running your custom Chef recipes\. 