# deploy Attributes<a name="attributes-json-deploy"></a>

If the attributes are associated with a [Deploy event](workingcookbook-events.md) or an [Execute Recipes stack command](workingstacks-commands.md), the `deploy` attribute contains an attribute for each app that was deployed, named by the app's short name\. Each app attribute contains the following attributes:


****  

|  |  |  | 
| --- |--- |--- |
| [application](#attributes-json-deploy-app-app) | [application\_type](#attributes-json-deploy-app-type) | [auto\_bundle\_on\_deploy](#attributes-json-deploy-app-auto) | 
| [database](#attributes-json-deploy-app-db) | [deploy\_to](#attributes-json-deploy-app-deploy-to) | [domains](#attributes-json-deploy-app-domains) | 
| [document\_root](#attributes-json-deploy-app-root) | [environment\_variables](#attributes-json-deploy-app-environment) | [group](#attributes-json-deploy-app-group) | 
| [keep\_releases](#attributes-json-deploy-app-keep-releases) | [memcached](#attributes-json-deploy-app-memcached) | [migrate](#attributes-json-deploy-app-migrate) | 
| [mounted\_at](#attributes-json-deploy-app-mounted) | [purge\_before\_symlink](#attributes-json-deploy-app-purge-before-symlink) | [rails\_env](#attributes-json-deploy-app-ssl-rails) | 
| [restart\_command](#attributes-json-deploy-app-restart) | [scm](#attributes-json-deploy-app-scm) | [ssl\_certificate](#attributes-json-deploy-app-ssl-cert) | 
| [ssl\_certificate\_ca](#attributes-json-deploy-app-ssl-ca) | [ssl\_certificate\_key](#attributes-json-deploy-app-ssl-key) | [ssl\_support](#attributes-json-deploy-app-ssl-supp) | 
| [stack](#attributes-json-deploy-app-stack) | [symlink\_before\_migrate](#attributes-json-deploy-app-symlink-migrate) | [symlinks](#attributes-json-deploy-app-symlinks) | 
| [user](#attributes-json-deploy-app-user) |  |  | 

**application**  <a name="attributes-json-deploy-app-app"></a>
The app's slug name, such as `"simplephp"` \(string\)\.  

```
node["deploy"]["appshortname"]["application"]
```

**application\_type**  <a name="attributes-json-deploy-app-type"></a>
The app type \(string\)\. Possible values are as follows:  
+ `java`: A Java app
+ `nodejs`: A Node\.js app
+ `php`: A PHP app
+ `rails`: A Ruby on Rails app
+ `web`: A static HTML page
+ `other`: All other application types

```
node["deploy"]["appshortname"]["application_type"]
```

**auto\_bundle\_on\_deploy**  <a name="attributes-json-deploy-app-auto"></a>
For Rails applications, whether to execute bundler during the deployment \(Boolean\)\.   

```
node["deploy"]["appshortname"]["auto_bundle_on_deploy"]
```

**database**  <a name="attributes-json-deploy-app-db"></a>
Contains the information required to connect the app's database\. If the app has an attached a database layer, AWS OpsWorks Stacks automatically assigns the appropriate values to these attributes\.    
**adapter**  
The database adapter, such as `mysql` \(string\)\.  

```
node["deploy"]["appshortname"]["database"]["adapter"]
```  
**database**  <a name="attributes-json-deploy-app-db-db"></a>
The database name, which is usually the app's slug name, such as `"simplephp"` \(string\)\.  

```
node["deploy"]["appshortname"]["database"]["database"]
```  
**data\_source\_provider**  
The data source: `mysql` or `rds` \(string\)\.  

```
node["deploy"]["appshortname"]["database"]["data_source_provider"]
```  
**host**  <a name="attributes-json-deploy-app-db-host"></a>
The database host's IP address \(string\)\.  

```
node["deploy"]["appshortname"]["database"]["host"]
```  
**password**  <a name="attributes-json-deploy-app-db-pwd"></a>
The database password \(string\)\.  

```
node["deploy"]["appshortname"]["database"]["password"]
```  
**port**  
The database port \(number\)\.  

```
node["deploy"]["appshortname"]["database"]["port"]
```  
**reconnect**  <a name="attributes-json-deploy-app-db-reconnect"></a>
For Rails applications, whether the application should reconnect if the connection no longer exists \(Boolean\)\.  

```
node["deploy"]["appshortname"]["database"]["reconnect"]
```  
**username**  <a name="attributes-json-deploy-app-db-user"></a>
The user name \(string\)\.  

```
node["deploy"]["appshortname"]["database"]["username"]
```

**deploy\_to**  <a name="attributes-json-deploy-app-deploy-to"></a>
Where the app is to be deployed to, such as `"/srv/www/simplephp"` \(string\)\.  

```
node["deploy"]["appshortname"]["deploy_to"]
```

**domains**  <a name="attributes-json-deploy-app-domains"></a>
A list of the app's domains \(list of string\)\.  

```
node["deploy"]["appshortname"]["domains"]
```

**document\_root**  <a name="attributes-json-deploy-app-root"></a>
The document root, if you specify a nondefault root, or null if you use the default root \(string\)\.  

```
node["deploy"]["appshortname"]["document_root"]
```

**environment\_variables**  <a name="attributes-json-deploy-app-environment"></a>
A collection of up to twenty attributes that represent the user\-specified environment variables that have been defined for the app\. For more information about how to define an app's environment variables, see [Adding Apps](workingapps-creating.md)\. Each attribute name is set to an environment variable name and the corresponding value is set to the variable's value, so you can use the following syntax to reference a particular value\.  

```
node["deploy"]["appshortname"]["environment_variables"]["variable_name"]
```

**group**  <a name="attributes-json-deploy-app-group"></a>
The app's group \(string\)\.  

```
node["deploy"]["appshortname"]["group"]
```

**keep\_releases**  <a name="attributes-json-deploy-app-keep-releases"></a>
The number of app deployments that AWS OpsWorks Stacks will store \(number\)\. This attribute controls the number of times you can roll back an app\. By default, it is set to the global value, [deploy\_keep\_releases ](attributes-recipes-deploy.md#attributes-recipes-deploy-global-keep-releases), which has a default value of 5\. You can override `keep_releases` to specify the number of stored deployments for a particular application\.  

```
node["deploy"]["appshortname"]["keep_releases"]
```

**memcached**  <a name="attributes-json-deploy-app-memcached"></a>
Contains two attributes that define the memcached configuration\.    
**host**  <a name="attributes-json-deploy-app-memcached-host"></a>
The Memcached server instance's IP address \(string\)\.  

```
node["deploy"]["appshortname"]["memcached"]["host"]
```  
**port**  <a name="attributes-json-deploy-app-memcached-port"></a>
The port that the memcached server is listening on \(number\)\.  

```
node["deploy"]["appshortname"]["memcached"]["port"]
```

**migrate**  <a name="attributes-json-deploy-app-migrate"></a>
For Rails applications, whether to run migrations \(Boolean\)\.  

```
node["deploy"]["appshortname"]["migrate"]
```

**mounted\_at**  <a name="attributes-json-deploy-app-mounted"></a>
The app's mount point, if you specify a nondefault mount point, or null if you use the default mount point \(string\)\.  

```
node["deploy"]["appshortname"]["mounted_at"]
```

**purge\_before\_symlink**  <a name="attributes-json-deploy-app-purge-before-symlink"></a>
For Rails apps, an array of paths to be cleared before creating symlinks \(list of string\)\.  

```
node["deploy"]["appshortname"]["purge_before_symlink"]
```

**rails\_env**  <a name="attributes-json-deploy-app-ssl-rails"></a>
For Rails App Server instances, the rails environment, such as `"production"` \(string\)\.  

```
node["deploy"]["appshortname"]["rails_env"]
```

**restart\_command**  <a name="attributes-json-deploy-app-restart"></a>
A command to be run when the app is restarted, such as `"echo 'restarting app'"`\.  

```
node["deploy"]["appshortname"]["restart_command"]
```

**scm**  <a name="attributes-json-deploy-app-scm"></a>
Contains a set of attributes that specify the information that OpsWorks uses to deploy the app from its source control repository\. The attributes vary depending on the repository type\.    
**password**  <a name="attributes-json-deploy-app-scm-pwd"></a>
The password, for private repositories, and null for public repositories \(string\)\. For private Amazon S3 buckets, the attribute is set to the secret key\.  

```
node["deploy"]["appshortname"]["scm"]["password"]
```  
**repository**  <a name="attributes-json-deploy-app-scm-repo"></a>
The repository URL, such as `"git://github.com/amazonwebservices/opsworks-demo-php-simple-app.git"` \(string\)\.  

```
node["deploy"]["appshortname"]["scm"]["repository"]
```  
**revision**  <a name="attributes-json-deploy-app-scm-revision"></a>
If the repository has multiple branches, the attribute specifies the app's branch or version, such as `"version1"` \(string\)\. Otherwise it is set to null\.  

```
node["deploy"]["appshortname"]["scm"]["revision"]
```  
**scm\_type**  <a name="attributes-json-deploy-app-scm-type"></a>
The repository type \(string\)\. Possible values are as follows:  
+ `"git"`: A Git repository
+ `"svn"`: A Subversion repository
+ `"s3"`: An Amazon S3 bucket
+ `"archive"`: An HTTP archive
+ `"other"`: Another repository type

```
node["deploy"]["appshortname"]["scm"]["scm_type"]
```  
**ssh\_key**  <a name="attributes-json-deploy-app-scm-key"></a>
A [deploy SSH key](workingapps-deploykeys.md), for accessing private Git repositories, and null for public repositories \(string\)\.  

```
node["deploy"]["appshortname"]["scm"]["ssh_key"]
```  
**user**  <a name="attributes-json-deploy-app-scm-user"></a>
The user name, for private repositories, and null for public repositories \(string\)\. For private Amazon S3 buckets, the attribute is set to the access key\.  

```
node["deploy"]["appshortname"]["scm"]["user"]
```

**ssl\_certificate**  <a name="attributes-json-deploy-app-ssl-cert"></a>
The app's SSL certificate, if you enabled SSL support, or null otherwise \(string\)\.  

```
node["deploy"]["appshortname"]["ssl_certificate"]
```

**ssl\_certificate\_ca**  <a name="attributes-json-deploy-app-ssl-ca"></a>
If SSL is enabled, an attribute for specifying an intermediate certificate authority key or client authentication \(string\)\.  

```
node["deploy"]["appshortname"]["ssl_certificate_ca"]
```

**ssl\_certificate\_key**  <a name="attributes-json-deploy-app-ssl-key"></a>
The app's SSL private key, if you enabled SSL support, or null otherwise \(string\)\.  

```
node["deploy"]["appshortname"]["ssl_certificate_key"]
```

**ssl\_support**  <a name="attributes-json-deploy-app-ssl-supp"></a>
Whether SSL is supported \(Boolean\)\.  

```
node["deploy"]["appshortname"]["ssl_support"]
```

**stack**  <a name="attributes-json-deploy-app-stack"></a>
Contains one Boolean attribute, `needs_reload`, that specifies whether to reload the app server during deployment\.  

```
node["deploy"]["appshortname"]["stack"]["needs_reload"]
```

**symlink\_before\_migrate**  <a name="attributes-json-deploy-app-symlink-migrate"></a>
For Rails apps, contains symlinks that are to be created before running migrations as `"link":"target"` pairs\.  

```
node["deploy"]["appshortname"]["symlink_before_migrate"]
```

**symlinks**  <a name="attributes-json-deploy-app-symlinks"></a>
Contains the deployment's symlinks as `"link":"target"` pairs\.  

```
node["deploy"]["appshortname"]["symlinks"]
```

**user**  <a name="attributes-json-deploy-app-user"></a>
The app's user \(string\)\.  

```
node["deploy"]["appshortname"]["user"]
```