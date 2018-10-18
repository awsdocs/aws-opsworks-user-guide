# Packaging Cookbook Dependencies Locally<a name="best-practices-packaging-cookbooks-locally"></a>

You can use Berkshelf to package your cookbook dependencies locally, upload the package to Amazon S3, and modify your stack to use the package on Amazon S3 as a cookbook source\. Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

The following walkthroughs describe how to pre\-package your cookbooks and their dependencies into a \.zip file, and then use the \.zip file as your cookbook source for Linux instances in AWS OpsWorks Stacks\. The first walkthrough describes how to package one cookbook\. The second walkthrough describes how to package multiple cookbooks\.

Before you begin, install the [Chef Development Kit](https://downloads.chef.io/chef-dk/) \(also known as Chef DK\), which is an assortment of tools built by the Chef community\. You will need this to use the `chef` command\-line tool\.

## Packaging Dependencies Locally in Chef 12<a name="best-practices-packaging-cookbooks-locally-12"></a>

In Chef 12 Linux, Berkshelf is no longer installed by default on stack instances\. We recommend that you install and use Berkshelf on a local development computer to package your cookbook dependencies locally\. Upload your package, with the dependencies included, to Amazon S3\. Finally, modify your Chef 12 Linux stack to use the uploaded package as a cookbook source\. Be aware of the following differences when you are packaging cookbooks in Chef 12\.

1. On the local computer, create a cookbook by running the `chef` command line tool\.

   ```
   chef generate cookbook "server-app"
   ```

   This command creates a cookbook, a Berksfile, a `metadata.rb` file, and a recipe directory, and places them in a folder that has the same name as the cookbook\. The following example shows the structure of what is created\.

   ```
   server-app <-- the cookbook you've just created
       └── Berksfile
       ├── metadata.rb
       └── recipes
   ```

1. In a text editor, edit the Berksfile to point to cookbooks on which the `server-app` cookbook will depend\. In our example, we want `server-app` to depend on the [https://supermarket.chef.io/cookbooks/java](https://supermarket.chef.io/cookbooks/java) cookbook from the Chef Supermarket\. We are specifying the version 1\.50\.0 or newer minor version, but you can enter any published version in the single quotation marks\. Save your changes and close the file\.

   ```
   source 'https://supermarket.chef.io'
   cookbook 'java', '~> 1.50.0'
   ```

1. Edit the `metadata.rb` file to add the dependency\. Save your changes and close the file\.

   ```
   depends 'java' , '~> 1.50.0'
   ```

1. Change to the `server-app` cookbook directory that Chef created for you, and then run the `package` command to create a `tar` file of the cookbook\. If you are packaging multiple cookbooks, you want to run this command at the root directory in which all cookbooks are stored\. To package a single cookbook, run this command at the cookbook directory level\. In this example, we run this command in the `server-app` directory\.

   ```
   berks package cookbooks.tar.gz
   ```

   The output resembles the following\. The `tar.gz` file is created in your local directory\.

   ```
   Cookbook(s) packaged to /Users/username/tmp/berks/cookbooks.tar.gz
   ```

1. In the AWS CLI, upload the package you just created to Amazon S3\. Make a note of the new URL of the cookbook package after you have uploaded it to S3; you'll need this URL for your stack settings\.

   ```
   aws s3 cp cookbooks.tar.gz s3://bucket-name/
   ```

   The output resembles the following\.

   ```
   upload: ./cookbooks.tar.gz to s3://bucket-name/cookbooks.tar.gz
   ```

1. In AWS OpsWorks Stacks, [modify your stack](https://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-installingcustom-enable.html) to use the package that you uploaded as the cookbook source\.

   1. Set the **Use custom Chef cookbooks** setting to **Yes**\.

   1. Set **Repository type** to **S3 Archive**\.

   1. In **Repository URL**, paste the URL of the cookbook package that you uploaded in step 5\.

   Save your stack changes\.

## Packaging Dependencies Locally for One Cookbook<a name="best-practices-packaging-cookbooks-locally-one"></a>

****

1. On the local computer, create a cookbook by using the chef command line tool: 

   ```
   chef generate cookbook "server-app"
   ```

   This command creates a cookbook and a Berksfile, and places them in a folder that has the same name as the cookbook\.

1.  Change to the cookbook directory that Chef created for you, and then package everything by running the following command: 

   ```
   berks package cookbooks.tar.gz
   ```

   The output looks like this:

   ```
   Cookbook(s) packaged to /Users/username/tmp/berks/cookbooks.tar.gz
   ```

1.  In the AWS CLI, upload the package you just created to Amazon S3: 

   ```
   aws s3 cp cookbooks.tar.gz s3://bucket-name/
   ```

   The output looks like this:

   ```
   upload: ./cookbooks.tar.gz to s3://bucket-name/cookbooks.tar.gz
   ```

1.  In AWS OpsWorks Stacks, [modify your stack](https://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-installingcustom-enable.html) to use the package that you uploaded as the cookbook source\. 

## Packaging Dependencies Locally for Multiple Cookbooks<a name="best-practices-packaging-cookbooks-locally-multiple"></a>

This example creates two cookbooks and packages the dependencies for them\.

1.  On the local computer, run the following `chef` commands to generate two cookbooks: 

   ```
   chef generate cookbook "server-app"
   chef generate cookbook "server-utils"
   ```

   In this example, the server\-app cookbook performs Java configurations, so we need to add a dependency on Java\.

1.  Edit `server-app/metadata.rb` to add a dependency on the community Java cookbook: 

   ```
   maintainer "The Authors"
   maintainer_email "you@example.com"
   license "all_rights"
   description "Installs/Configures server-app"
   long_description "Installs/Configures server-app"
   version "0.1.0"
   depends "java"
   ```

1.  Tell Berkshelf what to package by editing the Berksfile file in the cookbook root directory as follows: 

   ```
   source "https://supermarket.chef.io"
   cookbook "server-app", path: "./server-app"
   cookbook "server-utils", path: "./server-utils"
   ```

   Your file structure now looks like this:

   ```
    .. 
       └── Berksfile
       ├── server-app
       └── server-utils
   ```

1.  Finally, create a zip package, upload it to Amazon S3, and modify your AWS OpsWorks Stacks stack to use the new cookbook source\. To do this, follow steps 2 through 4 in [Packaging Dependencies Locally for One Cookbook](#best-practices-packaging-cookbooks-locally-one)\. 

## Additional resources<a name="w4ab1c11c41c15c15"></a>

For more information about packaging cookbook dependencies, see the following\.
+ [How to Package Cookbook Dependencies Locally with Berkshelf](https://aws.amazon.com/blogs/devops/how-to-package-cookbook-dependencies-locally-with-berkshelf/) on the AWS DevOps Blog
+ [Linux Chef 12 with Berkshelf](https://forums.aws.amazon.com/thread.jspa?threadID=221131) on the AWS OpsWorks forums
+ [Berkshelf in Chef 12](https://forums.aws.amazon.com/message.jspa?messageID=694464) on the AWS OpsWorks forums
+ [Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md) in this guide
+ [Cookbook Repositories](workingcookbook-installingcustom-repo.md) in this guide