# Using an External Cookbook on a Linux Instance: Berkshelf<a name="cookbooks-101-opsworks-berkshelf"></a>

**Note**  
Berkshelf is available only for Chef 11\.10 Linux stacks\.

Before you start implementing a cookbook, check out [Chef Community Cookbooks](https://github.com/opscode-cookbooks), which contains cookbooks that have been created by members of the Chef community for a wide variety of purposes\. Many of these cookbooks can be used with AWS OpsWorks Stacks without modification, so you might able to take advantage of them for some of your tasks instead of implementing all the code yourself\.

To use an external cookbook on an instance, you need a way to install it and manage any dependencies\. The preferred approach is to implement a cookbook that supports a dependency manager named Berkshelf\. Berkshelf works on Amazon EC2 instances, including AWS OpsWorks Stacks instances, but it is also designed to work with Test Kitchen and Vagrant\. However, the usage on Vagrant is a bit different than with AWS OpsWorks Stacks, so this topic includes examples for both platforms\. For more information on how to use Berkshelf, see [Berkshelf](http://berkshelf.com/)\.

**Topics**
+ [Using Berkshelf with Test Kitchen and Vagrant](#cookbooks-101-opsworks-berkshelf-vagrant)
+ [Using Berkshelf with AWS OpsWorks Stacks](#opsworks-berkshelf-opsworks)

## Using Berkshelf with Test Kitchen and Vagrant<a name="cookbooks-101-opsworks-berkshelf-vagrant"></a>

 This example shows how to use Berkshelf to install the getting\-started community cookbook and execute its recipe, which installs a brief text file in your home directory on the instance\.

**To install Berkshelf and initialize a cookbook**

1. On your workstation, install the Berkshelf gem, as follows\.

   ```
   gem install berkshelf
   ```

   Depending on your workstation, this command might require `sudo`, or you can also use a Ruby environment manager such as [RVM](https://rvm.io/)\. To verify that Berkshelf was successfully installed, run `berks --version`\.

1. The cookbook for this topic is named external\_cookbook\. You can use Berkshelf to create an initialized cookbook instead of the manual approach that the previous topics have taken\. To do so, navigate to the `opsworks_cookbooks` directory and run the following command\.

   ```
   berks cookbook external_cookbook
   ```

   The command creates the `external_cookbook` directory and several standard Chef and Test Kitchen subdirectories, including `recipes` and `test`\. The command also creates default versions of a number of standard files, including the following:
   + `metadata.rb`
   + Configuration files for Vagrant, Test Kitchen, and Berkshelf
   + An empty `default.rb` recipe in the `recipes` directory
**Note**  
You don't need to run `kitchen init`; the `berks cookbook` command handles those tasks\.

1. Run `kitchen converge`\. The newly created cookbook doesn't do anything interesting at this point, but it does converge\.

**Note**  
You can also use `berks init` to initialize an existing cookbook to use Berkshelf\.

To use Berkshelf to manage a cookbook's external dependencies, the cookbook's root directory must contain a `Berksfile`, which is a configuration file that specifies how Berkshelf should manage dependencies\. When you used `berks cookbook` to create the `external_cookbook` cookbook, it created a `Berksfile` with the following contents\.

```
source "https://supermarket.chef.io"
metadata
```

This file has the following declarations:
+ `source` – The URL of a cookbook source\.

  A Berksfile can have any number of `source` declarations, each of which specifies a default source for dependent cookbooks\. If you do not explicitly specify a cookbook's source, Berkshelf looks in the default repositories for a cookbook with the same name\. The default Berksfile includes a single `source` attribute which specifies the community cookbook repository\. That repository contains the getting\-started cookbook, so you can leave the line unchanged\.
+ `metadata` – Directs Berkshelf to include cookbook dependencies that are declared in the cookbook's `metadata.rb` file\.

  You can also declare a dependent cookbook in the Berksfile by including a `cookbook` attribute, as discussed later\.

There are two ways to declare a cookbook dependency:
+ By including a `cookbook` declaration in the Berksfile\.

  This is the approach used by AWS OpsWorks Stacks\. For example to specify the getting\-started cookbook used in this example, include `cookbook "getting-started"` in the Berksfile\. Berkshelf will then look in the default repositories for a cookbook with that name\. You can also use `cookbook` to explicitly specify a cookbook source, and even a particular version\. For more information, see [Berkshelf](http://berkshelf.com/)\.
+ By including a `metadata` declaration in the Berksfile and declaring the dependency in `metadata.rb`\.

  This declaration directs Berkshelf to include cookbook dependencies that are declared in `metadata.rb`\. For example, to declare a getting\-started dependency, add a `depends 'getting-started'` declaration to the cookbook's `metadata.rb` file\.

This example uses the first approach, for consistency with AWS OpsWorks Stacks\.

**To install the getting\-started cookbook**

1. Edit the default Berksfile to replace the `metadata` declaration with a `cookbook` declaration for `getting-started`\. The contents should look like the following\.

   ```
   source "https://supermarket.chef.io"
   
   cookbook 'getting-started'
   ```

1. Run `berks install`, which downloads the getting\-started cookbook from the community cookbook repository to your workstation's Berkshelf directory, which is typically `~/.berkshelf`\. This directory is often simply called *the Berkshelf*\. Look in the Berkshelf's `cookbooks` directory, and you should see the directory for the getting\-started cookbook, which will be named something like `getting-started-0.4.0`\.

1. Replace `external_cookbook::default` in the `.kitchen.yml` run list with `getting-started::default`\. This example doesn't run any recipes from external\_cookbook; it's basically just a way to use the getting\-started cookbook\. The `.kitchen.yml` file should now look like the following\.

   ```
   ---
   driver:
     name: vagrant
   
   provisioner:
     name: chef_solo
   
   platforms:
     - name: ubuntu-12.04
   
   suites:
     - name: default
       run_list:
         - recipe[getting-started::default]
       attributes:
   ```

1. Run `kitchen converge` and then use `kitchen login` to log in to the instance\. The login directory should contain a file named `chef-getting-started.txt` with something like the following:

   ```
   Welcome to Chef!
   
   This is Chef version 11.12.8.
   Running on ubuntu.
   Version 12.04.
   ```

   Test Kitchen installs cookbooks in the instance's `/tmp/kitchen/cookbooks` directory\. If you list the contents of that directory, you will see two cookbooks: external\_cookbook and getting\-started\.

1. Run `kitchen destroy` to shut down the instance\. The next example uses an AWS OpsWorks Stacks instance\.

## Using Berkshelf with AWS OpsWorks Stacks<a name="opsworks-berkshelf-opsworks"></a>

AWS OpsWorks Stacks optionally supports Berkshelf for Chef 11\.10 stacks\. To use Berkshelf with your stack, you must do the following\.
+ Enable Berkshelf for the stack\.

  AWS OpsWorks Stacks then handles the details of installing Berkshelf on the stack's instances\.
+ Add a Berksfile to your cookbook repository's root directory\.

  The Berksfile should contain `source` and `cookbook` declarations for all dependent cookbooks\.

When AWS OpsWorks Stacks installs your custom cookbook repository on an instance, it uses Berkshelf to install the dependent cookbooks that are declared in the repository's Berksfile\. For more information, see [Using Berkshelf](workingcookbook-chef11-10.md#workingcookbook-chef11-10-berkshelf)\.

This example shows how to use Berkshelf to install the getting\-started community cookbook on an AWS OpsWorks Stacks instance\. It also installs a version of the createfile custom cookbook, which creates a file in a specified directory\. For more information on how createfile works, see [Installing a File from a Cookbook](cookbooks-101-basics-files.md#cookbooks-101-basics-files-cookbook_file)\.

**Note**  
If this is the first time you have installed a custom cookbook on an AWS OpsWorks Stacks stack, you should first go through the [Running a Recipe on a Linux Instance](cookbooks-101-opsworks-opsworks-instance.md) example\.

Start by creating a stack, as summarized in the following\. For more information, see [Create a New Stack](workingstacks-creating.md)\.

**Create a stack**

1. Open the [AWS OpsWorks Stacks console](https://console.aws.amazon.com/opsworks/) and click **Add Stack**\.

1. Specify the following settings, accept the defaults for the other settings, and click **Add Stack**\.
   + **Name** – BerksTest
   + **Default SSH key** – An Amazon EC2 key pair

   If you need to create an Amazon EC2 key pair, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. Note that the key pair must belong to the same AWS region as the instance\. The example uses the default US West \(Oregon\) region\.

1. Click **Add a layer** and [add a custom layer](workinglayers-custom.md) to the stack with the following settings\.
   + **Name** – BerksTest
   + **Short name** – berkstest

   You could actually use any layer type for this example\. However, the example doesn't require any of the packages that are installed by the other layers, so a custom layer is the simplest approach\.

1. [Add a 24/7 instance](workinginstances-add.md) to the BerksTest layer with default settings, but don't start it yet\.

With AWS OpsWorks Stacks, cookbooks must be in a remote repository with a standard directory structure\. You then provide the download information to AWS OpsWorks Stacks, which automatically downloads the repository to each of the stack's instances on startup\. For simplicity, the repository for this example is a public Amazon S3 archive, but AWS OpsWorks Stacks also supports HTTP archives, Git repositories, and Subversion repositories\. For more information, see [Cookbook Repositories](workingcookbook-installingcustom-repo.md)\.

Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**To create the cookbook repository**

1. In your `opsworks_cookbooks` directory, create a directory named `berkstest_cookbooks`\. If you prefer, you can create this directory anywhere that you find convenient, because you will upload it to a repository\.

1. Add a file named Berksfile to `berkstest_cookbooks` with the following contents\. 

   ```
   source "https://supermarket.chef.io"
   
   cookbook 'getting-started'
   ```

   This file declares the getting\-started cookbook dependency, and directs Berkshelf to download it from the community cookbook site\.

1. Add a `createfile` directory to `berkstest_cookbooks` that contains the following\.
   + A `metadata.rb` file with the following contents\.

     ```
     name "createfile"
     version "0.1.0"
     ```
   + A `files/default` directory that contains an `example_data.json` file with the following content\.

     ```
     {
       "my_name" : "myname",
       "your_name" : "yourname",
       "a_number" : 42,
       "a_boolean" : true
     }
     ```

     The file's name and content are arbitrary\. The recipe simply copies the file to the specified location\.
   + A `recipes` directory that contains a `default.rb` file with the following recipe code\.

     ```
     directory "/srv/www/shared" do
       mode 0755
       owner 'root'
       group 'root'
       recursive true
       action :create
     end
     
     cookbook_file "/srv/www/shared/example_data.json" do
       source "example_data.json"
       mode 0644
       action :create_if_missing
     end
     ```

     This recipe creates `/srv/www/shared` and copies `example_data.json` to that directory from the cookbook's `files` directory\.

1. Create a `.zip` archive of `berkstest_cookbooks`, [Upload the archive to an Amazon S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html), [make the archive public](http://docs.aws.amazon.com/AmazonS3/latest/UG/EditingPermissionsonanObject.html), and record the archive's URL\. It should look something like `https://s3.amazonaws.com/cookbook_bucket/berkstest_cookbooks.zip`\.

You can now install the cookbooks and run the recipe\.

**To install the cookbooks and run the recipes**

1. [Edit the stack to enable custom cookbooks](workingcookbook-installingcustom-enable.md), and specify the following settings\.
   + **Repository type** – **Http Archive**
   + **Repository URL** – The cookbook archive URL that you recorded earlier
   + **Manage Berkshelf** – **Yes**

   The first two settings provide AWS OpsWorks Stacks with the information it needs to download the cookbook repository to your instances\. The last setting enables Berkshelf support, which downloads the getting\-started cookbook to the instance\. Accept the default values for the other settings and click **Save** to update the stack configuration\. 

1. Edit the BerksTest layer to [add the following recipes to the layer's Setup lifecycle event](workingcookbook-assigningcustom.md)\.
   + `getting-started::default`
   + `createfile::default`

1. [Start the instance](workinginstances-starting.md)\. The Setup event occurs after the instance finishes booting\. AWS OpsWorks Stacks then installs the cookbook repository, uses Berkshelf to download the getting\-started cookbook, and runs the layer's setup and deploy recipes, including `getting-started::default` and `createfile::default`\.

1. After the instance is online, [use SSH to log in](workinginstances-ssh.md)\. You should see the following
   + `/srv/www/shared` should contain `example_data.json`\.
   + `/root` should contain `chef-getting-started.txt`\.

     AWS OpsWorks Stacks runs recipes as root, so getting\-started installs the file in the `/root` directory rather than your home directory\.