# Learning More: Explore the Cookbook Used in This Walkthrough<a name="gettingstarted-linux-explore-cookbook"></a>

This topic describes the cookbook that AWS OpsWorks Stacks used for the walkthrough\.

A *cookbook* is a Chef concept\. Cookbooks are archive files that contain configuration information, such as recipes, attribute values, files, templates, libraries, definitions, and custom resources\. A *recipe* is also a Chef concept\. Recipes are instructions, written with Ruby language syntax, that specify the resources to use and the order in which those resources are applied\. For more information, go to [About Cookbooks](https://docs.chef.io/cookbooks.html) and [About Recipes](https://docs.chef.io/recipes.html) on the [Learn Chef](https://learn.chef.io/) website\.

To see the contents of the cookbook used in this walkthrough, extract the contents of the [opsworks\-linux\-demo\-cookbooks\-nodejs\.tar\.gz](https://s3.amazonaws.com/opsworks-demo-assets/opsworks-linux-demo-cookbooks-nodejs.tar.gz) file to an empty directory on your local workstation\. \(You can also log in to the instance that you deployed the cookbook to and explore the contents of the `/var/chef/cookbooks` directory\.\)

The `default.rb` file in the `cookbooks/nodejs_demo/recipes` directory is where the cookbook runs its code: 

```
app = search(:aws_opsworks_app).first
app_path = "/srv/#{app['shortname']}"

package "git" do
  options "--force-yes" if node["platform"] == "ubuntu" && node["platform_version"] == "18.04"
end

application app_path do
  javascript "4"
  environment.update("PORT" => "80")

  git app_path do
    repository app["app_source"]["url"]
    revision app["app_source"]["revision"]
  end

  link "#{app_path}/server.js" do
    to "#{app_path}/index.js"
  end

  npm_install
  npm_start
end
```

Here's what the file does:
+ `search(:aws_opsworks_app).first` uses Chef search to look up information about the app that will eventually be deployed to the instance\. This information includes settings such as the app's short name and its source repository details\. Because only one app was deployed in this walkthrough, Chef search gets these settings from the first item of information within the `aws_opsworks_app` search index on the instance\. Whenever an instance is launched, AWS OpsWorks Stacks stores this and other related information as a set of data bags on the instance itself, and you get the data bag contents through Chef search\. Although you can hard code these settings into this recipe, using data bags and Chef search is a more robust approach\. For more information about data bags, see the [AWS OpsWorks Stacks Data Bag Reference](data-bags.md)\. See also [About Data Bags](https://docs.chef.io/data_bags.html) on the [Learn Chef](https://learn.chef.io/) website\. For more information about Chef search, go to [About Search](https://docs.chef.io/chef_search.html) on the [Learn Chef](https://learn.chef.io/) website\.
+ The `package` resource installs Git on the instance\.
+ The `application` resource describes and deploys web applications:
  + `javascript` is the version of the JavaScript runtime to install\.
  + `environment` sets an environment variable\.
  + `git` gets the source code from the specified repository and branch\.
  + `app_path` is the path to clone the repository to\. If the path doesn't exist on the instance, AWS OpsWorks Stacks creates it\.
  + `link` creates a symbolic link\.
  + `npm_install` installs Node Package Manager, the default package manager for Node\.js\.
  + `npm_start` runs Node\.js\.

Although AWS OpsWorks Stacks created the cookbook used for this walkthrough, you can create your own cookbooks\. To learn how, see [Getting Started: Cookbooks](gettingstarted-cookbooks.md)\. Also, go to [About Cookbooks](https://docs.chef.io/cookbooks.html), [About Recipes](https://docs.chef.io/recipes.html), and [Learn the Chef Basics on Ubuntu](https://learn.chef.io/modules/learn-the-basics/ubuntu#/) on the [Learn Chef](https://learn.chef.io/) website, and the "Our first Chef cookbook" section in [First steps with Chef](http://gettingstartedwithchef.com/first-steps-with-chef.html) on the [Getting started with Chef](http://gettingstartedwithchef.com/) website\.