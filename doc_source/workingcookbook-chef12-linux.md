# Implementing Recipes for Chef 12 Stacks<a name="workingcookbook-chef12-linux"></a>

Chef 12 stacks provide the following advantages over Chef 11\.10 stacks:
+ Chef runs use Ruby 2\.1\.6, so your recipes can use the new Ruby syntax\.
+ Chef 12 stacks can use even more community cookbooks without modification\. Without any built\-in cookbooks in the way, there will no longer be any chance of name conflicts between built\-in cookbooks and custom cookbooks\. 
+ You are no longer limited to the Berkshelf versions that AWS OpsWorks Stacks has provided pre\-built packages for\. Berkshelf is no longer installed on AWS OpsWorks Stacks instances in Chef 12\. Instead, you can use any Berkshelf version on your local workstation\. 
+ There is now a clear separation between the built\-in cookbooks that AWS OpsWorks Stacks provides with Chef 12 \(Elastic Load Balancing, Amazon RDS, and Amazon ECS\) and custom cookbooks\. This makes troubleshooting failed Chef runs easier\.