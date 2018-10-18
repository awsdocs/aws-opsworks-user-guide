# Chef Versions<a name="workingcookbook-chef11"></a>

AWS OpsWorks Stacks supports multiple versions of Chef\. You select the version when you [create the stack](workingstacks-creating.md)\. AWS OpsWorks Stacks then installs that version of Chef on all of the stack's instances along with a set of built\-in recipes that are compatible with that version\. If you install any custom recipes, they must be compatible with the stack's Chef version\.

AWS OpsWorks Stacks currently supports Chef versions 12, 11\.10, 11\.4 and 0\.9 for Linux stacks, and Chef 12\.2 \(currently Chef 12\.22\) for Windows stacks\. For convenience, they are usually referred to by just their major and minor version numbers\. For Linux stacks, you can use the Configuration Manager to specify which Chef version to use when you [create a stack](workingstacks-creating.md)\. Windows stacks must use Chef 12\.2\. For more information, including guidelines for migrating stacks to more recent Chef versions, see [Chef Versions](#workingcookbook-chef11)\. For complete version information, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.

**Chef 12\.2**  
Chef 12\.2 support was introduced in May 2015, and is used only by Windows stacks\. The current version of Chef on Windows stacks is Chef 12\.22\. It runs with Ruby 2\.3\.6, and uses [chef\-client in local mode](https://docs.chef.io/ctl_chef_client.html#run-in-local-mode), which launches a local in\-memory Chef server called [chef\-zero](https://docs.chef.io/ctl_chef_client.html#about-chef-zero)\. The presence of this server enables recipes to use Chef search and data bags\. The support has some limitations, which are described in [Implementing Recipes: Chef 12\.2](workingcookbook-chef12.md), but you can run many community cookbooks without modification\.

**Chef 12**  
Chef 12 support was introduced in December 2015, and is used only by Linux stacks\. It runs with Ruby 2\.1\.6 or 2\.2\.3 and uses [chef\-client in local mode](https://docs.chef.io/ctl_chef_client.html#run-in-local-mode), which enables recipes to use Chef search and data bags\. For more information, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.

**Chef 11\.10**  
Chef 11\.10 support was introduced in March 2014, and is used only by Linux stacks\. It runs with Ruby 2\.0\.0 and uses [chef\-client in local mode](https://docs.chef.io/ctl_chef_client.html#run-in-local-mode), which enables recipes to use Chef search and data bags\. The support has some limitations, which are described in [Implementing Recipes: Chef 11\.10](workingcookbook-chef11-10.md), but you can run many community cookbooks without modification\. You can also use [Berkshelf](http://berkshelf.com/) to manage your cookbook dependencies\. The supported Berkshelf versions depend on the operating system\. For more information, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\. You cannot create CentOS stacks that use Chef 11\.10\.

**Chef 11\.4**  
Chef 11\.4 support was introduced in July 2013, and is used only by Linux stacks\. It runs with Ruby 1\.8\.7 and uses [chef\-solo](https://docs.chef.io/chef_solo.html), which does not support Chef search or data bags\. You can often use community cookbooks that depend on those features with AWS OpsWorks Stacks, but you must modify them as described in [Migrating to a new Chef Version](workingcookbook-chef11-migrate.md)\. You cannot create CentOS stacks that use Chef 11\.4\. Chef 11\.4 stacks are not supported in regional endpoints outside the US East \(N\. Virginia\) Region\.

**Chef 0\.9**  
 Chef 0\.9 is used only by Linux stacks and is no longer supported\. Note these details:   
+ You cannot use the console to create a new Chef 0\.9 stack\.

  You must use the CLI or API, or you must create a stack with a different Chef version and then edit the stack configuration\.
+ New AWS OpsWorks Stacks features are not available for Chef 0\.9 stacks\.
+ New operating system versions will provide only limited support for Chef 0\.9 stacks\.

  In particular, Amazon Linux 2014\.09 and later versions do not support Chef 0\.9 stacks with Rails App Server layers that depend on Ruby 1\.8\.7\.
+ New AWS regions, including EU \(Frankfurt\), do not support Chef 0\.9 stacks\.
We do not recommend using Chef 0\.9 for new stacks\. You should migrate any existing stacks to the latest Chef version as soon as possible\.

If you want to use community cookbooks with AWS OpsWorks Stacks, we recommend [that you specify Chef 12](workingstacks-creating.md) for new Linux stacks and migrate your existing Linux stacks to Chef 12\. You can use the AWS OpsWorks Stacks console, API, or CLI to migrate your existing stacks to a newer Chef version\. For more information, see [Migrating to a new Chef Version](workingcookbook-chef11-migrate.md)\.

**Topics**
+ [Implementing Recipes for Chef 12\.2 Stacks](workingcookbook-chef12.md)
+ [Implementing Recipes for Chef 12 Stacks](workingcookbook-chef12-linux.md)
+ [Implementing Recipes for Chef 11\.10 Stacks](workingcookbook-chef11-10.md)
+ [Implementing Recipes for Chef 11\.4 Stacks](workingcookbook-chef11-4.md)
+ [Migrating an Existing Linux Stack to a new Chef Version](workingcookbook-chef11-migrate.md)