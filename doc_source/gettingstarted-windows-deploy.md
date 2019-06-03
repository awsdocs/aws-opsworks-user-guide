# Step 2\.5: Deploy an App<a name="gettingstarted-windows-deploy"></a>

The IIS installation creates an `C:\inetpub\wwwroot` directory for your application's code and related files\. The next step is to install an app in that directory\. For this example, you will install a static HTML home page, `default.html`, in `C:\inetpub\wwwroot`\. You can easily extend the general approach to handle more complex scenarios, such as ASP\.NET applications\.

You could include the application's files in your cookbook and have `install.rb` copy them to `C:\inetpub\wwwroot`\. For examples of how to do this, see [Example 6: Creating Files](cookbooks-101-basics-files.md)\. However, this approach is not very flexible or efficient, and it is usually better to separate cookbook development from application development\.

The preferred solution is to implement a separate deployment recipe that retrieves the application's code and related files from a repository—any repository you prefer, not just the cookbook repository—and installs it on each IIS server instance\. This approach separates cookbook development from application development and, when you need to update your app, it allows you to just run the deployment recipe again without having to update your cookbooks\.

This topic shows how to implement a simple deployment recipe that deploys `default.htm` to your IIS server\. You can readily extend this example to more complex applications\.

**Topics**
+ [Create the Application and Store It in a Repository](#w4ab1c11c39c15c21c23c13)
+ [Implement a Recipe to Deploy the Application](#w4ab1c11c39c15c21c23c15)
+ [Update the Instance's Cookbooks](#w4ab1c11c39c15c21c23c17)
+ [Add the Recipe to the Custom IIS Layer](#w4ab1c11c39c15c21c23c19)
+ [Add an App](#w4ab1c11c39c15c21c23c21)
+ [Deploy the App and Open the Application](#w4ab1c11c39c15c21c23c23)

## Create the Application and Store It in a Repository<a name="w4ab1c11c39c15c21c23c13"></a>

You can use any repository you prefer for your applications\. For simplicity, this example stores `default.htm` in a public S3 bucket\.

**To create the application**

1. Create a directory named `iis-application` in a convenient location on your workstation\.

1. Add a `default.htm` file to `iis-application` with the following content\.

   ```
   <!DOCTYPE html>
   <html>
     <head>
       <title>IIS Example</title>
     </head>
     <body>
       <h1>Hello World!</h1>
     </body>
   </html>
   ```

1. [Create an S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html), [upload `default.htm` to the bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html), and record the URL for later use\. For simplicity, [make the file public](http://docs.aws.amazon.com/AmazonS3/latest/gsg/OpeningAnObject.html)\. 
**Note**  
This is a very simple application, but you can extend the basic principles to handle production\-level applications\.  
For more complex applications with multiple files, it is usually simpler to create a \.zip archive of `iis-application` and upload it to your S3 bucket\.  
You can then download the \.zip file and extract the contents to the appropriate directory\. There's no need to download multiple files, create a directory structure, or so on\.
For a production application, you will probably want to keep your files private\. For an example of how to have a recipe download files from a private S3 bucket, see [Using the SDK for Ruby on an AWS OpsWorks Stacks Windows Instance](cookbooks-101-opsworks-s3-windows.md)\.
You can store your application in any suitable repository\.  
You typically download the application by using a repository's public API\. This example uses the Amazon S3 API\. If, for example, you store your application on GitHub, you can use the [GitHub API](https://developer.github.com/guides/getting-started/)\.

## Implement a Recipe to Deploy the Application<a name="w4ab1c11c39c15c21c23c15"></a>

Add a recipe named `deploy.rb` to the `iis-cookbook` `recipes` directory, with the following contents\.

```
chef_gem "aws-sdk-s3" do
  compile_time false
  action :install
end

ruby_block "download-object" do
  block do
    require 'aws-sdk-s3'

    #1  
    # Aws.config[:ssl_ca_bundle] = 'C:\ProgramData\Git\bin\curl-ca-bundle.crt'
    Aws.use_bundled_cert!

    #2  
    query = Chef::Search::Query.new
    app = query.search(:aws_opsworks_app, "type:other").first
    s3region = app[0][:environment][:S3REGION]
    s3bucket = app[0][:environment][:BUCKET]
    s3filename = app[0][:environment][:FILENAME]

    #3  
    s3_client = Aws::S3::Client.new(region: s3region)
    s3_client.get_object(bucket: s3bucket,
                         key: s3filename,
                         response_target: 'C:\inetpub\wwwroot\default.htm')
  end 
  action :run
end
```

This example uses [SDK for Ruby v2](http://docs.aws.amazon.com/sdkforruby/api/index.html) to download the file\. However, AWS OpsWorks Stacks does not install this SDK on Windows instances, so the recipe starts with [https://docs.chef.io/chef/resources.html#chef-gem](https://docs.chef.io/chef/resources.html#chef-gem) resource, which handles that task\.

**Note**  
The `chef_gem` resource installs gems into Chef's dedicated Ruby version, which is the version that recipes use\. If you want to install a gem for a system\-wide Ruby version, use the [gem\_package](https://docs.chef.io/chef/resources.html#gem-package) resource\.

The bulk of the recipe is a [https://docs.chef.io/chef/resources.html#ruby-block](https://docs.chef.io/chef/resources.html#ruby-block) resource, which runs a block of Ruby code that uses the SDK for Ruby to download `default.htm`\. The code in the `ruby_block` can be divided into the following sections, which correspond to the numbered comments in the code example\. 

**1: Specify a Certificate Bundle**  
Amazon S3 uses SSL, so you need an appropriate certificate to download objects from an S3 bucket\. SDK for Ruby v2 does not include a certificate bundle, so you must provide one and configure the SDK for Ruby to use it\. AWS OpsWorks Stacks does not install a certificate bundle directly, but it does install Git, which includes a certificate bundle \(`curl-ca-bundle.crt`\)\. For convenience, this example configures the SDK for Ruby to use the Git certificate bundle for SSL\. You also can install your own bundle and configure the SDK accordingly\.

**2: Retrieve the Repository Data**  
To download an object from Amazon S3, you need the AWS region, bucket name, and key name\. As described later, this example provides this information by associating a set of environment variables with the app\. When you deploy an app, AWS OpsWorks Stacks adds a set of attributes to the instance's node object\. These attributes are essentially a hash table that contains the app configuration, including the environment variables\. The app attributes for this application will look something like the following, in JSON format\.   

```
{
  "app_id": "8f71a9b5-de7f-451c-8505-3f35086e5bb3",
  "app_source": {
      "password": null,
      "revision": null,
      "ssh_key": null,
      "type": "other",
      "url": null,
      "user": null
  },
  "attributes": {
      "auto_bundle_on_deploy": true,
      "aws_flow_ruby_settings": {},
      "document_root": null,
      "rails_env": null
  },
  "data_sources": [{"type": "None"}],
  "domains": ["iis_example_app"],
  "enable_ssl": false,
  "environment": {
      "S3REGION": "us-west-2",
      "BUCKET": "windows-example-app",
      "FILENAME": "default.htm"
  },
  "name": "IIS-Example-App",
  "shortname": "iis_example_app",
  "ssl_configuration": {
      "certificate": null,
      "private_key": null,
      "chain": null
  },
  "type": "other",
  "deploy": true
}
```
The app's environment variables are stored in the `[:environment]` attribute\. To retrieve them, use a Chef search query to retrieve the app's hash table, which is under the `aws_opsworks_app` node\. This app will be defined as the `other` type, so the query searches for apps of that type\. The recipe takes advantage of the fact that there is only one app on this instance, so the hash table of interest is just `app[0]`\. For convenience, the recipe then assigns the region, bucket, and file names to variables\.  
For more information about how to use Chef search, see \.[Obtaining Attribute Values with Chef Search](cookbooks-101-opsworks-opsworks-stack-config-search.md)

**3: Download the file**  
The third part of the recipe creates an [S3 client object](http://docs.aws.amazon.com/sdkforruby/api/Aws/S3/Client.html) and uses its `[get\_object](http://docs.aws.amazon.com/sdkforruby/api/Aws/S3/Client.html#get_object-instance_method)` method to download `default.htm` to the instance's `C:\inetpub\wwwroot` directory\. 

**Note**  
A recipe is a Ruby application, so Ruby code doesn't necessarily have to be in a `ruby_block`\. However, the code in the body of the recipe runs first, followed by the resources, in order\. For this example, if you put the download code in the recipe body, it would fail because the `chef_gem` resource wouldn't have installed the SDK for Ruby yet\. The code in the `ruby_block` resource executes when the resource executes, after the `chef_gem` resource has installed the SDK for Ruby\.

## Update the Instance's Cookbooks<a name="w4ab1c11c39c15c21c23c17"></a>

AWS OpsWorks Stacks automatically installs custom cookbooks on new instances\. However, you are working with an existing instance, so you must update your cookbook manually\.

**To update the instance's cookbooks**

1. Create a `.zip` archive of `iis-cookbook`, and upload it to the S3 bucket\.

   This overwrites the existing cookbook, but the URL stays the same, so you don't need to update the stack configuration\.

1. If your instance is not online, restart it\.

1. After the instance is online, choose **Stack** in the navigation pane, and then choose **Run Command**\.

1. For **Command**, choose [Update Custom Cookbooks](workingstacks-commands.md)\. This command installs the updated cookbook on the instance\.

1. Choose **Update Custom Cookbooks**\. The command might take a few minutes to finish\.

## Add the Recipe to the Custom IIS Layer<a name="w4ab1c11c39c15c21c23c19"></a>

As with `install.rb`, the preferred way to handle deployment is to assign `deploy.rb` to the appropriate lifecycle event\. You usually assign deployment recipes to the Deploy event, and they are referred to collectively as Deploy recipes\. Assigning a recipe to the Deploy event does not trigger the event\. Instead:
+ For new instances, AWS OpsWorks Stacks automatically runs the Deploy recipes after the Setup recipes have finished, so new instances automatically have the current application version\.
+ For online instances, you use a [deploy command](workingapps-deploying.md) to manually install new or updated applications\.

  This command triggers a Deploy event on the stack's instances, which runs the Deploy recipes\.

**To assign deploy\.rb to the layer's Deploy event**

1. Choose **Layers** in the navigation pane, and then choose **Recipes** under **Layer IISExample**\.

1. Under **Custom Chef Recipes**, add **iis\-cookbook::deploy** to the **Deploy** recipes box and choose **\+** to add the recipe to the layer\.

1. Choose **Save** to save the new configuration\. The custom Deploy recipes should now include `iis-cookbook::deploy`\.

## Add an App<a name="w4ab1c11c39c15c21c23c21"></a>

The final task is to add an app to the stack to represent your application in the AWS OpsWorks Stacks environment\. An app includes metadata such as the application's display name, and the data that is required to download the app from its repository\.

**To add the app to the stack**

1. Choose **Apps** in the navigation pane, and then choose **Add an app**\.

1. Configure the app with the following settings\.
   + **Name** – I**IIS\-Example\-App**
   + **Repository Type** – **Other**
   + **Environment Variables** – Add the following three environment variables:
     + **S3REGION** – The bucket's region \(in this case, `us-west-1`\)\.
     + **BUCKET** – The bucket name, such as `windows-example-app`\.
     + **FILENAME** – The file name: **default\.htm**\.

1. Accept default values for the remaining settings, and then choose **Add App** to add the app to the stack\.

**Note**  
This example uses environment variables to provide the download data\. An alternative approach is to use an S3 Archive repository type and provide the file's URL\. AWS OpsWorks Stacks adds the information, along with optional data, such as your AWS credentials, to the app's `app_source` attribute\. Your deploy recipe must get the URL from the app attributes and parse it to extract the region, bucket name, and file name\.

## Deploy the App and Open the Application<a name="w4ab1c11c39c15c21c23c23"></a>

AWS OpsWorks Stacks automatically deploys apps to new instances, but not to online instances\. Because your instance is already running, you must deploy the app manually\.

**To deploy the app**

1. Choose **Apps** in the navigation pane, and then choose **deploy** in the app's **Actions** column\.

1. **Command** should be set to **Deploy**\. Choose **Deploy** at the lower right of the **Deploy App** page\. The command might take a few minutes to finish\.

   After deployment is finished, you return to the **Apps** page\. The **Status** indicator shows **successful** in green, and the app name has a green check mark next to it to indicate a successful deployment\.

**Note**  
Windows apps are always the **Other** app type, so deploying the app does the following:  
Adds the app's data to the [stack configuration and deployment attributes](workingcookbook-json.md), as described earlier\.
Triggers a Deploy event on the stack's instances, which runs your custom Deploy recipes\.

**Note**  
For more information about how to troubleshoot failed deployments or applications, see [Debugging Recipes](troubleshoot-debug.md)\.

The app is now installed\. You can open it by choosing **Instances** in the **Navigation** pane, and then choosing the instance's public IP address\. This sends an HTTP request to the instance, and you should see something like the following in your browser\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/windows-iis-app.png)