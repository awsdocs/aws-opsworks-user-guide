# Next Steps<a name="cookbooks-101-basics-next"></a>

This chapter walked you through the basics of how to implement Chef cookbooks, but there's much more:
+ The examples showed you how to use some of the more commonly used resources, but there are many more\. 

  For the resources that were covered, the examples used only some of the available attributes and actions\. For a complete reference, see [About Resources and Providers](https://docs.chef.io/resource.html)\.
+ The examples used only the core cookbook elements: `recipes`, `attributes`, `files`, and `templates`\.

  Cookbooks can also include a variety of other elements, such as `libraries`, `definitions`, and `specs`\. For more information, see the [Chef documentation](https://docs.chef.io)\.
+ The examples used Test Kitchen only as a convenient way to start instances, run recipes, and log in to instances\.

  Test Kitchen is primarily a testing platform that you can use to run a variety of tests on your recipes\. If you haven't done so already, go through the rest of the [Test Kitchen walkthrough](http://kitchen.ci/docs/getting-started/), which introduces you to its testing features\.
+ [Implementing Cookbooks for AWS OpsWorks Stacks](cookbooks-101-opsworks.md) provides some more advanced examples, and shows how to implement cookbooks for AWS OpsWorks Stacks\.