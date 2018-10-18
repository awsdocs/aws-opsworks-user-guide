# Using Chef Deployment Hooks<a name="workingcookbook-extend-hooks"></a>

You can customize deployment by implementing a custom recipe to perform the required tasks and assigning it to the appropriate layer's Deploy event\. An alternative and sometimes simpler approach—especially if you don't need to implement a cookbook for other purposes—is to use Chef deployment hooks to run your customization code\. In addition, custom Deploy recipes run after the deployment has already been performed by the built\-in recipes\. Deployment hooks allow you to interact during a deployment, for example, after the app's code is checked out of the repository but before Apache is restarted\.

Chef deploys apps in four stages:
+ **Checkout**–Downloads the files from the repository
+ **Migrate**–Runs a migration, as required
+ **Symlink**–Creates symlinks
+ **Restart**–Restarts the application

Chef deployment hooks provide a simple way to customize a deployment by optionally running a user\-supplied Ruby application after each stage completes\. To use deployment hooks, implement one or more Ruby applications and place them in your app's `/deploy` directory\. \(If your app does not have a `/deploy` directory, create one at the `APP_ROOT` level\.\) The application must have one of the following names, which determines when it runs\.
+ `before_migrate.rb` runs after the Checkout stage is complete but before Migrate\.
+ `before_symlink.rb` runs after the Migrate stage is complete but before Symlink\.
+ `before_restart.rb` runs after the Symlink stage is complete but before Restart\.
+ `after_restart.rb` runs after the Restart stage is complete\.

Chef deployment hooks can access the node object by using standard node syntax, just like recipes\. Deployment hooks can also access the values of any [app environment variables](workingapps-creating.md#workingapps-creating-environment) that you have specified\. However, you must use `new_resource.environment["VARIABLE_NAME"] ` to access the variable's value instead of `ENV["VARIABLE_NAME"]`\. For more information, see [deploy](https://docs.chef.io/resource_deploy.html)\. 

For more information on how to use deployment hooks and what information a hook can access, see [Deploy Phases](https://docs.chef.io/resource_deploy.html#deploy-phases)\.