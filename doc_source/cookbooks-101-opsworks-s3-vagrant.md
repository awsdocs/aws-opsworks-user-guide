# Using the SDK for Ruby on a Vagrant Instance<a name="cookbooks-101-opsworks-s3-vagrant"></a>

This topic describes how a recipe running on a Vagrant instance can use the [AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/) to download a file from Amazon S3\. Before starting, you must first have a set of AWS credentials—an access key and a secret access key—that allow the recipe to access Amazon S3\.

**Important**  
We strongly recommend that you do not use root account credentials for this purpose\. Instead, create an IAM user with an appropriate policy and provide those credentials to the recipe\. For more information, see [Best Practices for Managing AWS Access Keys](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.  
Be careful not to put credentials—even IAM user credentials—in a publicly accessible location, such as by uploading a file containing the credentials to a public GitHub or Bitbucket repository\. Doing so exposes your credentials and could compromise your account's security\.  
 Recipes running on an EC2Amazon EC2 instance can use an even better approach, an IAM role, as described in [Using the SDK for Ruby on an AWS OpsWorks Stacks Linux Instance](cookbooks-101-opsworks-s3-opsworks.md)\.  
Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

If you don't already have an appropriate IAM user, you can create one as follows\. For more information, see [What Is IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html)\.

**To create an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users** and, if necessary, choose **Create New Users** to create a new IAM user for the administrative user\.

1. Type a user name, select **Generate an access key for each user**, and choose **Create**\.

1. Choose **Download Credentials**, save the credentials file to a convenient location on your system, and choose **Close**\.

1. On the **Users** page, choose the new user's name and then choose **Attach Policy**\.

1. Type **S3** in the **Policy Type** search box to display the Amazon S3 policies\. Select **AmazonS3ReadOnlyAccess** and choose **Attach Policy**\. If you prefer, you can specify a policy that grants broader permissions, such as **AmazonS3FullAccess**, but standard practice is to grant only those permissions that are required\. In this case, the recipe will only be downloading a file, so read\-only access is sufficient\.

You must next provide a file to be downloaded\. This example assumes that you will put a file named `myfile.txt` in a newly created S3 bucket named `cookbook_bucket`\. 

**To provide a file for downloading**

1. Create a file named `myfile.txt` with the following text and save it in a convenient location on your workstation\.

   ```
   This is the file that you just downloaded from Amazon S3.
   ```

1. On the [Amazon S3 console](https://console.aws.amazon.com/s3/), create a bucket named `cookbook_bucket` in the ** Standard** region and upload `myfile.txt` to the bucket\.

Set the cookbook up as follows\.

**To set up the cookbook**

1. Create a directory within `opsworks_cookbooks` named `s3bucket` and navigate to it\.

1. Initialize and configure Test Kitchen, as described in [Example 1: Installing Packages](cookbooks-101-basics-packages.md)\.

1. Replace the text in `.kitchen.yml` with the following\.

   ```
   ---
   driver:
     name: vagrant
   
   provisioner:
     name: chef_solo
     environments_path: ./environments
   
   platforms:
     - name: ubuntu-14.04
   
   suites:
     - name: s3bucket
       provisioner:
         solo_rb:
           environment: test
       run_list:
         - recipe[s3bucket::default]
       attributes:
   ```

1. Add two directories to `s3bucket`: `recipes` and `environments`\.

1. Create an environment file named `test.json` with the following `default_attributes` section, replacing the `access_key` and `secret_key` values with the corresponding keys for your IAM user\. Save the file to the cookbook's `environments` folder\.

   ```
   {
     "default_attributes" : {
       "cookbooks_101" : {
         "access_key": "AKIAIOSFODNN7EXAMPLE",
         "secret_key" : "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
       }
     },
     "chef_type" : "environment",
     "json_class" : "Chef::Environment"
   }
   ```

You have a variety of ways to provide credentials to a recipe running on an instance\. The key consideration is limiting the chances of accidentally exposing the keys and compromising your account security\. For that reason, using explicit key values in your code is not recommended\. The example instead puts the key values in the node object, which allows the recipe to reference them by using node syntax instead of exposing literal values\. You must have root privileges to access the node object, which limits the possibility that the keys might be exposed\. For more information, see [Best Practices for Managing AWS Access Keys](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.

**Note**  
Notice that the example uses nested attributes, with `cookbooks_101` as the first element\. This practice limits the chance of a name collision if there are other `access_key` or `secret_key` attributes in the node object\.

The following recipe downloads `myfile.text` from the `cookbook_bucket` bucket\.

```
gem_package "aws-sdk ~> 3" do
  action :install
end

ruby_block "download-object" do
  block do
    require 'aws-sdk'

    s3 = Aws::S3::Client.new(
          :access_key_id => "#{node['cookbooks_101']['access_key']}",
          :secret_access_key => "#{node['cookbooks_101']['secret_key']}")

    myfile = s3.bucket['cookbook_bucket'].objects['myfile.txt']
    Dir.chdir("/tmp")
    File.open("myfile.txt", "w") do |f|
      f.write(myfile.read)
      f.close
    end
  end
  action :run
end
```

The first part of the recipe installs the SDK for Ruby, which is a gem package\. The [gem\_package](https://docs.chef.io/chef/resources.html#gem-package) resource installs gems that will be used by recipes or other applications\.

**Note**  
Your instance usually has two Ruby instances, which are typically different versions\. One is a dedicated instance that is used by the Chef client\. The other is used by applications and recipes running on the instance\. It's important to understand this distinction when installing gem packages, because there are two resources for installing gems, [gem\_package](https://docs.chef.io/chef/resources.html#gem-package) and [chef\_gem](https://docs.chef.io/chef/resources.html#chef-gem)\. If applications or recipes use the gem package, install it with `gem_package`\. `chef_gem` is only for gem packages used by Chef client\.

The remainder of the recipe is a [ruby\_block](https://docs.chef.io/chef/resources.html#ruby-block) resource, which contains the Ruby code that downloads the file\. You might think that because a recipe is a Ruby application, you could put the code in the recipe directly\. However, a Chef run compiles all of that code before executing any resources\. If you put the example code directly in the recipe, Ruby will attempt to resolve the `require 'aws-sdk'` statement before it executes the `gem_package` resource\. Because the SDK for Ruby hasn't been installed yet, compilation will fail\.

Code in a `ruby_block` resource isn't compiled until that resource is executed\. In this example, the `ruby_block` resource is executed after the `gem_package` resource has finished installing the SDK for Ruby, so the code will run successfully\.

The code in the `ruby_block` works as follows\. 

1. Creates a new [https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/S3.html](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/S3.html) object, which provides the service interface\.

   The access and secret keys are specified by referencing the values stored in the node object\.

1. Calls the `S3` object's `bucket.objects` association, which returns an [https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/S3/Object.html](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/S3/Object.html) object named `myfile` that represents `myfile.txt`\.

1. Uses `Dir.chdir` to set the working directory to `/tmp`\.

1. Opens a file named `myfile.txt`, writes the contents of `myfile` to the file, and closes the file\.

**To run the recipe**

1. Create a file named `default.rb` with the example recipe and save it to the `recipes` directory\.

1. Run `kitchen converge`\.

1. Run `kitchen login` to log in to the instance, and then run `ls /tmp`\. You should see the `myfile.txt`, along with several Test Kitchen files and directories\.

   ```
   vagrant@s3bucket-ubuntu-1204:~$ ls /tmp
   install.sh  kitchen  myfile.txt  stderr
   ```

   You can also run `cat /tmp/myfile.txt` to verify that the file's content is correct\.

When you are finished, run `kitchen destroy` to terminate the instance\.