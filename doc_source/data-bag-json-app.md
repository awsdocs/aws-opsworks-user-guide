# App Data Bag \(aws\_opsworks\_app\)<a name="data-bag-json-app"></a>

For a [Deploy event](workingcookbook-events.md) or an [Execute Recipes stack command](workingstacks-commands.md), represents an app's settings\.

The following example shows how to use Chef search to search through a single data bag item and then multiple data bag items to write messages to the Chef log with the apps' short names and source URLs:

```
app = search("aws_opsworks_app").first
Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")

search("aws_opsworks_app").each do |app|
  Chef::Log.info("********** The app's short name is '#{app['shortname']}' **********")
  Chef::Log.info("********** The app's URL is '#{app['app_source']['url']}' **********")
end
```


****  

|  |  |  | 
| --- |--- |--- |
| [app\_id](#data-bag-json-app-app-id) | [app\_source](#data-bag-json-app-app-source) | [data\_sources](#data-bag-json-app-app-data-source) | 
| [deploy](#data-bag-json-app-deploy) | [attributes](#data-bag-json-app-attributes) | [domains](#data-bag-json-app-app-domains) | 
| [enable\_ssl](#data-bag-json-app-enable-ssl) | [environment](#data-bag-json-app-app-environment) | [name](#data-bag-json-app-app-name) | 
| [shortname](#data-bag-json-app-app-shortname) | [ssl\_configuration](#data-bag-json-app-app-ssl-config) | [type](#data-bag-json-app-app-type) | 

**app\_id**  <a name="data-bag-json-app-app-id"></a>
The app ID \(string\)\. A GUID that identifies the app\.

**app\_source**  <a name="data-bag-json-app-app-source"></a>
A set of content that specifies the information that AWS OpsWorks Stacks uses to deploy the app from its source control repository\. The content varies depending on the repository type\.    
**password**  
The password for private repositories, and `"null"` for public repositories \(string\)\. For private  S3 buckets, this content is set to the secret key\.  
**revision**  
If the repository has multiple branches, the content specifies the app's branch or version, such as `"version1"` \(string\)\. Otherwise, it is set to `"null"`\.  
**ssh\_key**  
A [deploy SSH key](workingapps-deploykeys.md) for accessing private Git repositories, and `"null"` for public repositories \(string\)\.  
**type**  
The app's source location \(string\)\. Valid values include:  
+ `"archive"`
+ `"git"`
+ `"other"`
+ `"s3"`  
**url**  
Where the app source is located \(string\)\.  
**user**  
The user name for private repositories, and `"null"` for public repositories \(string\)\. For private S3 buckets, the content is set to the access key\.

**attributes**  <a name="data-bag-json-app-attributes"></a>
A set of content that describes the directory structure and content of the app\.    
**document\_root**  <a name="data-bag-json-app-documentroot"></a>
The root directory of the document tree\. Defines the path to the document root–or the location of the app's home page, such as `home_html`–that is relative to your deployment directory\. Unless this attribute is specified, the document\_root defaults to `public`\. The value of `document_root` can start only with `a-z`, `A-Z`, `0-9`, `_` \(underscore\) or `-` \(hyphen\) characters\.

**data\_sources**  <a name="data-bag-json-app-app-data-source"></a>
The information required to connect to the app's database\. If the app has an attached database layer, AWS OpsWorks Stacks automatically assigns the appropriate values to this content\.  
The value of data\_sources is an array, and arrays are accessed by an integral offset, not by key\. For example, to access the app's first data source, use `app[:data_sources][0][:type]`\.    
**database\_name**  
The database name, which is typically the app's short name \(string\)\.  
**type**  
The database instance's type, typically `"RdsDbInstance"` \(string\)\.  
**arn**  
The database instance's Amazon Resource Name \(ARN\) \(string\)\.

**deploy**  <a name="data-bag-json-app-deploy"></a>
Whether the app should be deployed \(Boolean\)\. `true` for apps that should be deployed in a Deploy lifecycle event\. In a Setup lifecycle event, this content will be `true` for all apps\. To determine which apps should be deployed on an instance, check the layers to which the instance belongs\.

**domains**  <a name="data-bag-json-app-app-domains"></a>
A list of the app's domains \(list of strings\)\.

**enable\_ssl**  <a name="data-bag-json-app-enable-ssl"></a>
Whether SSL support is enabled \(Boolean\)\.

**environment**  <a name="data-bag-json-app-app-environment"></a>
A collection of user\-specified environment variables that have been defined for the app\. For more information about how to define an app's environment variables, see [Adding Apps](workingapps-creating.md)\. Each content name is set to an environment variable name and the corresponding value is set to the variable's value\.

**name**  <a name="data-bag-json-app-app-name"></a>
The app's name, which is used for display purposes \(string\)\.

**shortname**  <a name="data-bag-json-app-app-shortname"></a>
The app's short name, which is generated by AWS OpsWorks Stacks from the name \(string\)\. The shortname is used internally and by recipes; it is used as the name of the directory where your app files are installed\.

**ssl\_configuration**  <a name="data-bag-json-app-app-ssl-config"></a>  
**certificate**  
If you enabled SSL support, the app's SSL certificate; otherwise, `"null"` \(string\)\.  
**chain**  
If SSL is enabled, content for specifying an intermediate certificate authority key or client authentication \(string\)\.  
**private\_key**  
If you enabled SSL support, the app's SSL private key; otherwise, `"null"` \(string\)\.

**type**  <a name="data-bag-json-app-app-type"></a>
The app's type, which is always set to `"other"` for Chef 12 Linux and Chef 12\.2 Windows stacks \(string\)\.