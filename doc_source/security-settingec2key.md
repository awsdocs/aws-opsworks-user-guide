# Installing an Amazon EC2 Key<a name="security-settingec2key"></a>

When you create a stack, you can specify an Amazon EC2 SSH key that is installed by default on every instance in the stack\.

![\[Default SSH key list on Add stack page\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/ec2_keys.png)

The **Default SSH key** list shows your AWS account's Amazon EC2keys\. You can do one of the following: 
+ Select the appropriate key from the list\.
+ Select **Do not use a default SSH key** to specify no key\.

If you selected **Do not use a default SSH key**, or you want to override a stack's default key, you can specify a key when you create an instance\.

![\[Specifying an SSH key\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/instance_keys.png)

When you start the instance AWS OpsWorks Stacks installs the public key in the `authorized_keys` file\.