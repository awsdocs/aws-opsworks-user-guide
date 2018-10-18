# Cookbook Repositories<a name="workingcookbook-installingcustom-repo"></a>

Your custom cookbooks must be stored in an online repository, either an archive such as a \.zip file or a source control manager such as Git\. A stack can have only one custom cookbook repository, but the repository can contain any number of cookbooks\. When you install or update the cookbooks, AWS OpsWorks Stacks installs the entire repository in a local cache on each of the stack's instances\. When an instance needs, for example, to run one or more recipes, it uses the code from the local cache\.

The following describes how to structure your cookbook repository, which depends on the type\. The italicized text in the illustrations represents user\-defined directory and file names, including the repository or archive name\.

**Source Control Manager**  
AWS OpsWorks Stacks supports the following source control managers:  
+ Linux stacks – Git and Subversion
+ Windows stacks – Git
The following shows the required directory and file structure:  

![\[Mandatory structure for SCM cookbook repositories\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cookbook_folders.png)
+ The cookbook directories must all be at the top\-level\.

**Archive**  
AWS OpsWorks Stacks supports the following archives:  
+ Linux stacks – zip, gzip, bzip2, or tarball files, stored on Amazon S3 or a web site \(HTTP archive\)\. 

  AWS OpsWorks Stacks does not support uncompressed tarballs\.
+ Windows stacks – zip and tgz \(gzip compressed tar\) files, stored on Amazon S3\.
The following shows the required directory and file structure, which depends on whether you are running a Linux or Windows stack\. The cookbook structure is the same as for SCM repositories, so it is represented by an ellipsis \(\.\.\.\)\.  

![\[Mandatory structure for archives\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/cookbook_folders_archive.png)
+ Linux stacks – The cookbook directories must be contained in a root directory\. 
+ Windows stacks – The cookbooks must be at the archive's top level\.

  If you have only one cookbook, you can optionally omit the cookbook directory and put the cookbook files at the top level\. In that case, AWS OpsWorks Stacks obtains the cookbook name from metadata\.rb\.

Each cookbook directory has least one and typically all of the following standard directories and files, which must use standard names:
+ `attributes` – The cookbook's attributes files\. 
+ `recipes` – The cookbook's recipe files\.
+ `templates` – The cookbook's template files\. 
+ *other* – Optional user\-defined directories that contain other file types, such as definitions or specs\.
+ `metadata.rb` – The cookbook's metadata\.

  For Chef 11\.10 and later, if your recipes depend on other cookbooks, you must include corresponding `depends` statements in your cookbook's `metadata.rb` file\. For example, if your cookbook includes a recipe with a statement such as `include_recipe anothercookbook::somerecipe`, your cookbook's `metadata.rb` file must include the following line: `depends "anothercookbook"`\. For more information, see [About Cookbook Metadata](http://docs.chef.io/cookbook_repo.html#about-cookbook-metadata)\.

Templates must be in a subdirectory of the `templates` directory, which contains at least one and optionally multiple subdirectories\. Those subdirectories can optionally have subdirectories as well\.
+ Templates usually have a `default` subdirectory, which contains the template files that Chef uses by default\.
+ *other* represents optional subdirectories that can be used for operating system\-specific templates\.
+ Chef automatically uses the template from the appropriate subdirectory, based on naming conventions that are described in [File Specificity](http://docs.chef.io/templates.html#file-specificity)\. For example, for the Amazon Linux and Ubuntu operating systems, you can put operating system\-specific templates in subdirectories named `amazon` or `ubuntu`, respectively\.

The details of how you handle custom cookbooks depend on your preferred repository type\. 

**To use an archive**

1. Implement your cookbooks by using the folder structure shown in the preceding section\.

1. Create a compressed archive and upload it to an Amazon S3 bucket or a website\.

   If you update your cookbooks, you must create and upload a new archive file\. Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**To use an SCM**

1. Set up a Git or Subversion repository using the structure shown earlier\.

1. Optionally, use the repository's version control features to implement multiple branches or versions\.

   If you update your cookbooks, you can do so in a new branch and just direct OpsWorks to use the new version\. You can also specify particular tagged versions\. For details, see [Specifying a Custom Cookbook Repository](workingcookbook-installingcustom-enable.md#workingcookbook-installingcustom-enable-repo)\.

[Installing Custom Cookbooks](workingcookbook-installingcustom-enable.md) describes how to have AWS OpsWorks Stacks install your cookbook repository on the stack's instances\.

**Important**  
After you update existing cookbooks in the repository, you must run the `update_cookbooks` stack command to direct AWS OpsWorks Stacks to update each online instance's local cache\. For more information, see [Run Stack Commands](workingstacks-commands.md)\.