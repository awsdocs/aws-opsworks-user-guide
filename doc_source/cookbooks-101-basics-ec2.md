# Example 9: Using Amazon EC2 Instances<a name="cookbooks-101-basics-ec2"></a>

To this point, you've been running instances locally in VirtualBox\. While this is quick and easy, you will eventually want to test your recipes on an Amazon EC2 instance\. In particular, if you want to run recipes on Amazon Linux, it is available only on Amazon EC2\. You can use a similar system such as CentOS for preliminary implementation and testing, but the only way to fully test your recipes on Amazon Linux is with an Amazon EC2 instance\. 

This topic shows how to run recipes on an Amazon EC2 instance\. You will use Test Kitchen and Vagrant in much the same way as the preceding sections, with two differences: 
+ The driver is [https://rubygems.org/gems/kitchen-ec2](https://rubygems.org/gems/kitchen-ec2) instead of Vagrant\.
+ The cookbook's `.kitchen.yml` file must be configured with the information required to launch the Amazon EC2 instance\.

**Note**  
An alternative approach is to use the `vagrant-aws` Vagrant plug\-in\. For more information, see [Vagrant AWS Provider](https://github.com/mitchellh/vagrant-aws)\.

You will need AWS credentials to create an Amazon EC2 instance\. If you don't have an AWS account you can obtain one, as follows\. 

**To sign up for AWS**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

You should then [create an IAM user](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html) with permissions to access Amazon EC2 and save the user's access and secret keys to a secure location on your workstation\. Test Kitchen will use those credentials to create the instance\. The preferred way to provide credentials to Test Kitchen is to assign the keys to the following environment variables on your workstation\.
+ AWS\_ACCESS\_KEY – your user's access key, which will look something like AKIAIOSFODNN7EXAMPLE\.
+ AWS\_SECRET\_KEY – your user's secret key, which will look something like wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY\.

This approach reduces the chances of accidentally compromising your account by, for example, uploading a project containing your credentials to a public repository\. For more information, see [Best Practices for Managing AWS Access Keys](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.

**To set up the cookbook**

1. To use the `kitchen-ec2` driver, you must have the `ruby-dev` package installed on your system\. The following example command shows how to use `aptitude` to install the package on a Ubuntu system\. 

   ```
   sudo aptitude install ruby1.9.1-dev 
   ```

1. The `kitchen-ec2` driver is a gem, which you can install as follows:

   ```
   gem install kitchen-ec2
   ```

   Depending on your workstation, this command might require `sudo`, or you can also use a Ruby environment manager such as [RVM](https://rvm.io/)\. This procedure was tested with version 0\.8\.0 of the `kitchen-ec2` driver, but there are newer versions\. To install a [specific version](https://rubygems.org/gems/kitchen-ec2/versions), run `gem install kitchen-ec2 -v <version number>`\.

1. You must specify an Amazon EC2 SSH key pair that Test Kitchen can use to connect to the instance\. If you don't have an Amazon EC2 key pair, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) for information on how to create one\. Note that the key pair must belong to the same AWS region as the instance\. The example uses US West \(N\. California\)\.

   After you have selected a key pair, create a subdirectory of `opsworks_cookbooks` named `ec2_keys` and copy the key pair's private key \(`.pem`\) file to that subdirectory\. Note that putting the private key in `ec2_keys` is just a convenience that simplifies the code a bit; it can be anywhere on your system\.

1. Create a subdirectory of `opsworks_cookbooks` named `createdir-ec2` and navigate to it\.

1. Add a `metadata.rb` file to `createdir-ec2` with the following content\.

   ```
   name "createdir-ec2"
   version "0.1.0"
   ```

1. Initialize Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md)\. The following section describes how to configure `.kitchen.yml`, which is significantly more complicated for Amazon EC2 instances\.

1. Add a `recipes` subdirectory to `createdir-ec2`\.

## Configuring \.kitchen\.yml for Amazon EC2<a name="w4ab1c11c63b7c13c15c29c23"></a>

You configure `.kitchen.yml` with the information that the `kitchen-ec2` driver needs to launch an appropriately configured Amazon EC2 instance\. The following is an example of a `.kitchen.yml` file for an Amazon Linux instance in the US West \(N\. California\) region\.

```
driver:
  name: ec2
  aws_ssh_key_id: US-East1
  region: us-west-1
  availability_zone: us-west-1c
  require_chef_omnibus: true
  security_group_ids: sg........
  subnet_id: subnet-.........
  associate_public_ip: true
  interface: dns

provisioner:
  name: chef_solo

platforms:
  -name: amazon
  driver:
    image_id: ami-xxxxxxxx
  transport:
    username: ec2-user
    ssh_key: ../ec2_keys/US-East1.pem

suites:
  - name: default
    run_list:
      - recipe[createdir-ec2::default]
    attributes:
```

You can use the default settings for the `provisioner` and `suites` sections, but you must modify the default `driver` and `platforms` settings\. This example uses a minimal list of settings, and accepts the default values for the remainder\. For a complete list of `kitchen-ec2` settings, see [Kitchen::Ec2: A Test Kitchen Driver for Amazon EC2](https://github.com/test-kitchen/kitchen-ec2)\.

The example sets the following `driver` attributes\. It assumes that you have assigned your user's access and secret keys to the standard environment variables, as discussed earlier\. The driver uses those keys by default\. Otherwise, you must explicitly specify the keys by adding `aws_access_key_id` and `aws_secret_access_key` to the `driver` attributes, set to the appropriate key values\.

**name**  
\(Required\) This attribute must be set to `ec2`\.

**aws\_ssh\_key\_id**  
\(Required\) The Amazon EC2 SSH key pair name, which is named `US-East1` in this example\. 

**transport\.ssh\_key**  
\(Required\) The private key \(`.pem`\) file for the key that you specified for `aws_ssh_key_id`\. For this example, the file is named `US-East1.pem` and is in the `../opsworks/ec2_keys` directory\.

**region**  
\(Required\) The instance's AWS region\. The example uses US West \(N\. California\), which is represented by `us-west-1`\)\.

**availability\_zone**  
\(Optional\) The instance's Availability Zone\. If you omit this setting, Test Kitchen uses a default Availability Zone for the specified region, which is `us-west-1b` for US West \(N\. California\)\. However, the default zone might not be available for your account\. In that case, you must explicitly specify an Availability Zone\. As it happens, the account used to prepare the examples doesn't support `us-west-1b`, so the example explicitly specifies `us-west-1c`\.

**require\_chef\_omnibus**  
When set to `true`, this setting ensures that the omnibus installer is used to install `chef-client` to all platform instances\.

**security\_group\_ids**  
\(Optional\) A list of security group IDs to apply to the instance\. This setting applies the `default` security group to the instance\. Make sure that the security group ingress rules allow inbound SSH connections, or Test Kitchen will not be able to communicate with the instance\. If you use the `default` security group, you might need to edit it accordingly\. For more information, see [Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)\.

**subnet\_id**  
The ID of the target subnet for the instance, if applicable\.

**associate\_public\_ip**  
You can have Amazon EC2 associate a public IP address with the instance if you want to be able to access the instance from the Internet\.

**interface**  
The host name configuration type that you use to access the instance\. Valid values are `dns`, `public`, `private`, or `private_dns`\. If you do not specify a value for this attribute, `kitchen-ec2` sets up the host name configuration in the following order\. If you omit this attribute, the configuration type is not set\.  

1. DNS name

1. Public IP address

1. Private IP address

1. Private DNS name

**Important**  
Rather than use your account credentials for the access and secret keys, you should create an IAM user and provide those credentials to Test Kitchen\. For more information, see [Best Practices for Managing AWS Access Keys](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.  
Be careful not to put `.kitchen.yml` in a publicly accessible location, such as uploading it to a public GitHub or Bitbucket repository\. Doing so exposes your credentials and could compromise your account's security\.

The `kitchen-ec2` driver provides default support for the following platforms:
+ ubuntu\-10\.04
+ ubuntu\-12\.04
+ ubuntu\-12\.10
+ ubuntu\-13\.04
+ ubuntu\-13\.10
+ ubuntu\-14\.04
+ centos\-6\.4
+ debian\-7\.1\.0
+ windows\-2012r2
+ windows\-2008r2

If you want to use one or more of these platforms, add the appropriate platform names to `platforms`\. The `kitchen-ec2` driver automatically selects an appropriate AMI and generates an SSH user name\. You can use other platforms—this example uses Amazon Linux—but you must explicitly specify the following `platforms` attributes\.

**name**  
The platform name\. This example uses Amazon Linux, so `name` is set to `amazon`\.

**driver**  
The `driver` attributes, which include the following:  
+ `image_id` – The platform's AMI, which must belong to the specified region\. The example uses `ami-ed8e9284`, an Amazon Linux AMI from the US West \(N\. California\) region\.
+ `transport.username` – The SSH user name that Test Kitchen will use to communicate with the instance\.

  Use `ec2-user` for Amazon Linux\. Other AMIs might have different user names\.

Replace the code in `.kitchen.yml` with the example, and assign appropriate values to account\-specific attributes such as `aws_access_key_id`\.

## Running the Recipe<a name="w4ab1c11c63b7c13c15c29c25"></a>

This example uses the recipe from [Iteration](cookbooks-101-basics-ruby.md#cookbooks-101-basics-ruby-iteration)\.

**To run the recipe**

1. Create a file named `default.rb` with the following code and save it to the cookbook's `recipes` folder\.

   ```
   directory "/srv/www/shared" do
     mode 0755
     owner 'root'
     group 'root'
     recursive true
     action :create
   end
   ```

1. Run `kitchen converge` to execute the recipe\. Note that this command will take longer to complete than the previous examples because of the time required to launch and initialize an Amazon EC2 instance\.

1.  Go to the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), select the US West \(N\. California\)\) region, and click **Instances** in the navigation pane\. You will see the newly created instance in the list\. 

1. Run `kitchen login` to log in to the instance, just as you have been doing for instances running in VirtualBox\. You will see the newly created directories under `/srv`\. You can also use your favorite SSH client to connect to the instance\.