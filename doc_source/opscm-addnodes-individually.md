# Add nodes individually<a name="opscm-addnodes-individually"></a>

This section describes how to run a `knife` command that adds, or *bootstraps*, an EC2 instance so that the Chef server can manage it\. 

The minimum supported version of `chef-client` on nodes associated with an AWS OpsWorks for Chef Automate server is 13\.*x*\. We recommend running the [most current, stable `chef-client` version](https://downloads.chef.io/chef/stable)\.

**Topics**
+ [\(Optional\) Specify the URL of your Chef Automate Server Root CA](#opscm-addnodes-customdomain)
+ [Supported Operating Systems](#w2ab1b9c26c15c11c11)
+ [Add Nodes with Knife](#w2ab1b9c26c15c11c13)
+ [More Information](#w2ab1b9c26c15c11c15)

## \(Optional\) Specify the URL of your Chef Automate Server Root CA<a name="opscm-addnodes-customdomain"></a>

If your server is using a custom domain and certificate, you might need to edit the `ROOT_CA_URL` variable in the userdata script with a public URL that you can use to get the root CA PEM\-formatted certificate of your server\. The following AWS CLI commands upload your root CA to an Amazon S3 bucket, and generate a presigned URL that you can use for one hour\.

1. Upload the root CA PEM\-formatted certificate to S3\.

   ```
   aws s3 cp ROOT_CA_PEM_FILE_PATH s3://bucket_name/
   ```

1. Generate a presigned URL that you can use for one hour \(3600 seconds, in this example\) to download the root CA\.

   ```
   aws s3 presign s3://bucket_name/ROOT_CA_PEM_FILE_NAME --expires-in 3600
   ```

1. Edit the variable `ROOT_CA_URL` in the userdata script with the value of the pre\-signed URL\.

## Supported Operating Systems<a name="w2ab1b9c26c15c11c11"></a>

For the current list of supported operating systems for nodes, see the [Chef website](https://docs.chef.io/platforms.html)\.

## Add Nodes with Knife<a name="w2ab1b9c26c15c11c13"></a>

The [https://github.com/chef/knife-ec2](https://github.com/chef/knife-ec2) plug\-in is included with Chef Workstation\. If you are more familiar with `knife-ec2`, you can use it instead of `knife bootstrap` to provision and bootstrap new EC2instances\. Otherwise, launch a new EC2 instance, and then follow the steps in this section\.

**To add nodes to manage**

1. Run the following `knife bootstrap` command\. This command bootstraps an EC2 instance to the nodes that your Chef server will manage\. Note that you are instructing the Chef server to run recipes from the `nginx` cookbook that you installed in [Use Policyfile\.rb to Get Cookbooks from a Remote Source](opscm-starterkit.md#install-cookbooks-policyfile)\. For more information about adding nodes by running the `knife bootstrap` command, see [Bootstrap a Node](https://docs.chef.io/install_bootstrap.html) in the Chef documentation\.

   The following table shows valid user names for node operating systems in the `knife` command in this step\. If neither `root` nor `ec2-user` works, check with your AMI provider\. For more information about connecting to Linux\-based instances, see [Connecting to Your Linux Instance Using SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) in the AWS documentation\.  
**Valid values for user names in node operating systems**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/opscm-addnodes-individually.html)

   ```
   knife bootstrap INSTANCE_IP_ADDRESS -N INSTANCE_NAME -x USER_NAME --sudo --run-list "recipe[nginx]"
   ```

1. Verify that the new node was added by running the following commands, replacing *INSTANCE\_NAME* with the name of the instance that you just added\.

   ```
   knife client show INSTANCE_NAME
   knife node show INSTANCE_NAME
   ```

## More Information<a name="w2ab1b9c26c15c11c15"></a>

Visit the [Learn Chef tutorial site](https://learn.chef.io/tutorials/manage-a-node/opsworks) to learn more about using AWS OpsWorks for Chef Automate servers and Chef Automate premium features\.