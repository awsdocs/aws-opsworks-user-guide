# Setup Recipes<a name="create-custom-setup"></a>

Setup recipes are assigned to the layer's Setup [lifecycle](workingcookbook-events.md) event and run after an instance boots\. They perform tasks such as installing packages, creating configuration files, and starting services\. After the Setup recipes finish running, AWS OpsWorks Stacks runs the [Deploy recipes](create-custom-deploy.md) to deploy any apps to the new instance\.

**Topics**
+ [tomcat::setup](#create-custom-setup-setup)
+ [tomcat::install](#create-custom-setup-install)
+ [tomcat::service](#create-custom-setup-service)
+ [tomcat::container\_config](#create-custom-setup-config)
+ [tomcat::apache\_tomcat\_bind](#create-custom-setup-bind)

## tomcat::setup<a name="create-custom-setup-setup"></a>

The `tomcat::setup` recipe is intended to be assigned to a layer's Setup lifecycle event\.

```
include_recipe 'tomcat::install'
include_recipe 'tomcat::service'

service 'tomcat' do
  action :enable
end

# for EBS-backed instances we rely on autofs
bash '(re-)start autofs earlier' do
  user 'root'
  code <<-EOC
    service autofs restart
  EOC
  notifies :restart, resources(:service => 'tomcat')
end

include_recipe 'tomcat::container_config'
include_recipe 'apache2'
include_recipe 'tomcat::apache_tomcat_bind'
```

`tomcat::setup` recipe is largely a metarecipe\. It includes a set of dependent recipes that handle most of the details of installing and configuring Tomcat and related packages\. The first part of `tomcat::setup` runs the following recipes, which are discussed later: 
+ The [tomcat::install](#create-custom-setup-install) recipe installs the Tomcat server package\.
+ The [tomcat::service](#create-custom-setup-service) recipe sets up the Tomcat service\.

The middle part of `tomcat::setup` enables and starts the Tomcat service:
+ The Chef [service resource](https://docs.chef.io/chef/resources.html#service) enables the Tomcat service at boot\.
+ The Chef [bash resource](https://docs.chef.io/chef/resources.html#bash) runs a Bash script to start the autofs daemon, which is necessary for Amazon EBS\-backed instances\. The resource then notifies the `service` resource to restart the Tomcat service\.

  For more information, see: [autofs](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/s2-nfs-config-autofs.html) \(for Amazon Linux\) or [Autofs](https://help.ubuntu.com/community/Autofs) \(for Ubuntu\)\.

The final part of `tomcat::setup` creates configuration files and installs and configures the front\-end Apache server:
+ The [tomcat::container\_config](#create-custom-setup-config) recipe creates configuration files\.
+ The `apache2` recipe \(which is shorthand for `apache2::default`\) is an AWS OpsWorks Stacks built\-in recipe that installs and configures an Apache server\.
+ The [tomcat::apache\_tomcat\_bind](#create-custom-setup-bind) recipe configures the Apache server to function as a front\-end for the Tomcat server\.

**Note**  
You can often save time and effort by using built\-in recipes to perform some of the required tasks\. This recipe uses the built in `apache2::default` recipe to install Apache rather than implementing it from scratch\. For another example of how to use built\-in recipes, see [Deploy Recipes](create-custom-deploy.md)\.

The following sections describe the Tomcat cookbook's Setup recipes in more detail\. For more information on the `apache2` recipes, see [opsworks\-cookbooks/apache2](https://github.com/aws/opsworks-cookbooks/tree/release-chef-11.4/apache2)\.

## tomcat::install<a name="create-custom-setup-install"></a>

The `tomcat::install `recipe installs the Tomcat server, the OpenJDK, and a Java connector library that handles the connection to the MySQL server\.

```
tomcat_pkgs = value_for_platform(
  ['debian', 'ubuntu'] => {
    'default' => ["tomcat#{node['tomcat']['base_version']}", 'libtcnative-1', 'libmysql-java']
  },
  ['centos', 'redhat', 'fedora', 'amazon'] => {
    'default' => ["tomcat#{node['tomcat']['base_version']}", 'tomcat-native', 'mysql-connector-java']
  },
  'default' => ["tomcat#{node['tomcat']['base_version']}"]
)

tomcat_pkgs.each do |pkg|
  package pkg do
    action :install
  end
end

link ::File.join(node['tomcat']['lib_dir'], node['tomcat']['mysql_connector_jar']) do
  to ::File.join(node['tomcat']['java_dir'], node['tomcat']['mysql_connector_jar'])
  action :create
end

# remove the ROOT webapp, if it got installed by default
include_recipe 'tomcat::remove_root_webapp'
```

The recipe performs the following tasks:

1. Creates a list of packages to be installed, depending on the instance's operating system\.

1. Installs each package in the list\.

   The Chef [package resource](https://docs.chef.io/chef/resources.html#id146) uses the appropriate provider—`yum` for Amazon Linux and `apt-get` for Ubuntu— to handle the installation\. The package providers install OpenJDK as a Tomcat dependency, but the MySQL connector library must be installed explicitly\.

1. Uses a Chef [link resource](https://docs.chef.io/chef/resources.html#link) to create a symlink in the Tomcat server's lib directory to the MySQL connector library in the JDK\.

   Using the default attribute values, the Tomcat lib directory is `/usr/share/tomcat6/lib` and the MySQL connector library \(`mysql-connector-java.jar`\) is in `/usr/share/java/`\.

The `tomcat::remove_root_webapp` recipe removes the ROOT web application \(`/var/lib/tomcat6/webapps/ROOT` by default\) to avoid some security issues\.

```
ruby_block 'remove the ROOT webapp' do
  block do
    ::FileUtils.rm_rf(::File.join(node['tomcat']['webapps_base_dir'], 'ROOT'), :secure => true)
  end
  only_if { ::File.exists?(::File.join(node['tomcat']['webapps_base_dir'], 'ROOT')) && !::File.symlink?(::File.join(node['tomcat']['webapps_base_dir'], 'ROOT')) }
end
```

The `only_if` statement ensures that the recipe removes the file only if it exists\.

**Note**  
The Tomcat version is specified by the `['tomcat']['base_version']` attribute, which is set to 6 in the attributes file\. To install Tomcat 7, you can use custom JSON attributes to override the attribute\. Just [edit your stack settings](workingstacks-edit.md) and enter the following JSON in the **Custom Chef JSON** box, or add it to any existing custom JSON:  

```
{
  'tomcat' : {
    'base_version' : 7
  }
}
```
The custom JSON attribute overrides the default attribute and sets the Tomcat version to 7\. For more information on overriding attributes, see [Overriding Attributes](workingcookbook-attributes.md)\.

## tomcat::service<a name="create-custom-setup-service"></a>

The `tomcat::service` recipe creates the Tomcat service definition\.

```
service 'tomcat' do
  service_name "tomcat#{node['tomcat']['base_version']}"

  case node[:platform]
  when 'centos', 'redhat', 'fedora', 'amazon'
    supports :restart => true, :reload => true, :status => true
  when 'debian', 'ubuntu'
    supports :restart => true, :reload => false, :status => true
  end

  action :nothing
end
```

The recipe uses the Chef [service resource](https://docs.chef.io/chef/resources.html#service) to specify the Tomcat service name \(tomcat6, by default\) and sets the `supports` attribute to define how Chef manages the service's restart, reload, and status commands on the different operating systems\.
+ `true` indicates that Chef can use the init script or other service provider to run the command
+ `false` indicates that Chef must attempt to run the command by other means\.

Notice that the `action` is set to `:nothing`\. For each lifecycle event, AWS OpsWorks Stacks initiates a [Chef run](https://docs.chef.io/chef_client_overview.html#the-chef-client-run) to execute the appropriate set of recipes\. The Tomcat cookbook follows a common pattern of having a recipe create the service definition, but not restart the service\. Other recipes in the Chef run handle the restart, typically by including a `notifies` command in the `template` resources that are used to create configuration files\. Notifications are a convenient way to restart a service because they do so only if the configuration has changed\. In addition, if a Chef run has multiple restart notifications for a service, Chef restarts the service at most once\. This practice avoids problems that can occur when attempting to restart a service that is not fully operational, which is a common source of Tomcat errors\.

 The Tomcat service must be defined for any Chef run that uses restart notifications\. `tomcat::service` is therefore included in several recipes, to ensure that the service is defined for every Chef run\. There is no penalty if a Chef run includes multiple instances of `tomcat::service` because Chef ensures that a recipe executes only once per run, regardless of how many times it is included\.

## tomcat::container\_config<a name="create-custom-setup-config"></a>

The `tomcat::container_config` recipe creates configuration files from cookbook template files\.

```
include_recipe 'tomcat::service'

template 'tomcat environment configuration' do
  path ::File.join(node['tomcat']['system_env_dir'], "tomcat#{node['tomcat']['base_version']}")
  source 'tomcat_env_config.erb'
  owner 'root'
  group 'root'
  mode 0644
  backup false
  notifies :restart, resources(:service => 'tomcat')
end

template 'tomcat server configuration' do
  path ::File.join(node['tomcat']['catalina_base_dir'], 'server.xml')
  source 'server.xml.erb'
  owner 'root'
  group 'root'
  mode 0644
  backup false
  notifies :restart, resources(:service => 'tomcat')
end
```

The recipe first calls `tomcat::service`, which defines the service if necessary\. The bulk of the recipe consists of two [template resources](https://docs.chef.io/chef/resources.html#template), each of which creates a configuration file from one of the cookbook's template files, sets the file properties, and notifies Chef to restart the service\.

### Tomcat Environment Configuration File<a name="create-custom-setup-config-env"></a>

The first `template` resource uses the `tomcat_env_config.erb` template file to create a Tomcat environment configuration file, which is used to set environment variables such as `JAVA_HOME`\. The default file name is the `template` resource's argument\. `tomcat::container_config` uses a `path` attribute to override the default value and name the configuration file `/etc/sysconfig/tomcat6` \(Amazon Linux\) or `/etc/default/tomcat6` \(Ubuntu\)\. The `template` resource also specifies the file's owner, group, and mode settings and directs Chef to not create backup files\.

If you look at the source code, there are actually three versions of `tomcat_env_config.erb`, each in a different subdirectory of the `templates` directory\. The `ubuntu` and `amazon` directories contain the templates for their respective operating systems\. The `default` folder contains a dummy template with a single comment line, which is used only if you attempt to run this recipe on an instance with an unsupported operating system\. The `tomcat::container_config` recipe doesn't need to specify which `tomcat_env_config.erb` to use\. Chef automatically picks the appropriate directory for the instance's operating system based on rules described in [File Specificity](http://docs.chef.io/templates.html#file-specificity)\.

The `tomcat_env_config.erb` files for this example consist largely of comments\. To set additional environment variables, just uncomment the appropriate lines and provide your preferred values\.

**Note**  
Any configuration setting that might change should be defined as an attribute rather than hardcoded in the template\. That way, you don't have to rewrite the template to change a setting, you can just override the attribute\.

The Amazon Linux template sets only one environment variable, as shown in the following excerpt\.

```
...
# Use JAVA_OPTS to set java.library.path for libtcnative.so
#JAVA_OPTS="-Djava.library.path=/usr/lib"

JAVA_OPTS="${JAVA_OPTS} <%= node['tomcat']['java_opts'] %>"

# What user should run tomcat
#TOMCAT_USER="tomcat"
...
```

JAVA\_OPTS can be used to specify Java options such as the library path\. Using the default attribute values, the template sets no Java options for Amazon Linux\. You can set your own Java options by overriding the `['tomcat']['java_opts']` attribute, for example, by using custom JSON attributes\. For an example, see [Create a Stack](create-custom-stack.md#create-custom-stack-stack)\.

The Ubuntu template sets several environment variables, as shown in the following template excerpt\.

```
# Run Tomcat as this user ID. Not setting this or leaving it blank will use the
# default of tomcat<%= node['tomcat']['base_version'] %>.
TOMCAT<%= node['tomcat']['base_version'] %>_USER=tomcat<%= node['tomcat']['base_version'] %>
...
# Run Tomcat as this group ID. Not setting this or leaving it blank will use
# the default of tomcat<%= node['tomcat']['base_version'] %>.
TOMCAT<%= node['tomcat']['base_version'] %>_GROUP=tomcat<%= node['tomcat']['base_version'] %>
...
JAVA_OPTS="<%= node['tomcat']['java_opts'] %>"

<% if node['tomcat']['base_version'].to_i < 7 -%>
# Unset LC_ALL to prevent user environment executing the init script from
# influencing servlet behavior.  See Debian bug #645221
unset LC_ALL
<% end -%>
```

Using default attribute values, the template sets the Ubuntu environment variables as follows:
+ `TOMCAT6_USER` and `TOMCAT6_GROUP`, which represent the Tomcat user and group, are both set to `tomcat6`\.

  If you set \['tomcat'\]\['base\_version'\] to `tomcat7`, the variable names resolve to `TOMCAT7_USER` and `TOMCAT7_GROUP`, and both are set to `tomcat7`\.
+ `JAVA_OPTS` is set to `-Djava.awt.headless=true -Xmx128m -XX:+UseConcMarkSweepGC`:
  + Setting `-Djava.awt.headless` to `true` informs the graphics engine that the instance is headless and does not have a console, which addresses faulty behavior of certain graphical applications\.
  + `-Xmx128m` ensures that the JVM has adequate memory resources, 128MB for this example\.
  + `-XX:+UseConcMarkSweepGC` specifies concurrent mark sweep garbage collection, which helps limit garbage\-collection induced pauses\.

    For more information, see: [Concurrent Mark Sweep Collector Enhancements](http://docs.oracle.com/javase/6/docs/technotes/guides/vm/cms-6.html)\.
+ If the Tomcat version is less than 7, the template unsets `LC_ALL`, which addresses a Ubuntu bug\.

**Note**  
With the default attributes, some of these environment variables are simply set to their default values\. However, explicitly setting environment variables to attributes means you can define custom JSON attributes to override the default attributes and provide custom values\. For more information on overriding attributes, see [Overriding Attributes](workingcookbook-attributes.md)\.

For the complete template files, see the [source code](https://github.com/amazonwebservices/opsworks-example-cookbooks/tree/master/tomcat)\.

### Server\.xml Configuration File<a name="create-custom-setup-config-server"></a>

The second `template` resource uses `server.xml.erb` to create the [`system.xml` configuration file](http://tomcat.apache.org/tomcat-7.0-doc/config/), which configures the servlet/JSP container\. `server.xml.erb` contains no operating system\-specific settings, so it is in the `template` directory's `default` subdirectory\.

The template uses standard settings, but it can create a `system.xml` file for either Tomcat 6 or Tomcat 7\. For example, the following code from the template's server section configures the listeners appropriately for the specified version\.

```
<% if node['tomcat']['base_version'].to_i > 6 -%>
  <!-- Security listener. Documentation at /docs/config/listeners.html
  <Listener className="org.apache.catalina.security.SecurityListener" />
  -->
<% end -%>
  <!--APR library loader. Documentation at /docs/apr.html -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!--Initialize Jasper prior to webapps are loaded. Documentation at /docs/jasper-howto.html -->
  <Listener className="org.apache.catalina.core.JasperListener" />
  <!-- Prevent memory leaks due to use of particular java/javax APIs-->
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
<% if node['tomcat']['base_version'].to_i < 7 -%>
  <!-- JMX Support for the Tomcat server. Documentation at /docs/non-existent.html -->
  <Listener className="org.apache.catalina.mbeans.ServerLifecycleListener" />
<% end -%>
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
<% if node['tomcat']['base_version'].to_i > 6 -%>
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
<% end -%>
```

The template uses attributes in place of hardcoded settings so you can easily change the settings by defining custom JSON attributes\. For example:

```
<Connector port="<%= node['tomcat']['port'] %>" protocol="HTTP/1.1"
           connectionTimeout="20000"
           URIEncoding="<%= node['tomcat']['uri_encoding'] %>"
           redirectPort="<%= node['tomcat']['secure_port'] %>" />
```

For more information, see the [source code](https://github.com/amazonwebservices/opsworks-example-cookbooks/tree/master/tomcat)\.

## tomcat::apache\_tomcat\_bind<a name="create-custom-setup-bind"></a>

The `tomcat::apache_tomcat_bind` recipe enables the Apache server to act as Tomcat's front end, receiving incoming requests and forwarding them to Tomcat and returning the responses to the client\. This example uses [mod\_proxy](https://httpd.apache.org/docs/2.2/mod/mod_proxy.html) as the Apache proxy/gateway\.

```
execute 'enable mod_proxy for apache-tomcat binding' do
  command '/usr/sbin/a2enmod proxy'
  not_if do
    ::File.symlink?(::File.join(node['apache']['dir'], 'mods-enabled', 'proxy.load')) || node['tomcat']['apache_tomcat_bind_mod'] !~ /\Aproxy/
  end
end

execute 'enable module for apache-tomcat binding' do
  command "/usr/sbin/a2enmod #{node['tomcat']['apache_tomcat_bind_mod']}"
  not_if {::File.symlink?(::File.join(node['apache']['dir'], 'mods-enabled', "#{node['tomcat']['apache_tomcat_bind_mod']}.load"))}
end

include_recipe 'apache2::service'

template 'tomcat thru apache binding' do
  path ::File.join(node['apache']['dir'], 'conf.d', node['tomcat']['apache_tomcat_bind_config'])
  source 'apache_tomcat_bind.conf.erb'
  owner 'root'
  group 'root'
  mode 0644
  backup false
  notifies :restart, resources(:service => 'apache2')
end
```

To enable `mod_proxy`, you must enable the `proxy` module and a protocol\-based module\. You have two options for the protocol module: 
+ HTTP: `proxy_http`
+ [Apache JServ Protocol](http://tomcat.apache.org/connectors-doc/ajp/ajpv13a.html) \(AJP\): `proxy_ajp`

  AJP is an internal Tomcat protocol\.

Both the recipe's [execute resources](https://docs.chef.io/chef/resources.html#execute) run the `a2enmod` command, which enables the specified module by creating the required symlinks:
+ The first `execute` resource enables the `proxy` module\.
+ The second `execute` resource enables the protocol module, which is set to `proxy_http` by default\.

  If you would rather use AJP, you can define custom JSON to override the `apache_tomcat_bind_mod` attribute and set it to `proxy_ajp`\. 

The `apache2::service` recipe is an AWS OpsWorks Stacks built\-in recipe that defines the Apache service\. For more information, see the [recipe](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.4/apache2/recipes/service.rb) in the AWS OpsWorks Stacks GitHub repository\. 

The `template` resource uses `apache_tomcat_bind.conf.erb` to create a configuration file, named `tomcat_bind.conf` by default\. It places the file in the `['apache']['dir']/.conf.d` directory\. The `['apache']['dir']` attribute is defined in the built\-in `apache2` attributes file, and is set by default to `/etc/httpd` \(Amazon Linux\), or `/etc/apache2` \(Ubuntu\)\. If the `template` resource creates or changes the configuration file, the `notifies` command schedules an Apache service restart\.

```
<% if node['tomcat']['apache_tomcat_bind_mod'] == 'proxy_ajp' -%>
ProxyPass <%= node['tomcat']['apache_tomcat_bind_path'] %> ajp://localhost:<%= node['tomcat']['ajp_port'] %>/
ProxyPassReverse <%= node['tomcat']['apache_tomcat_bind_path'] %> ajp://localhost:<%= node['tomcat']['ajp_port'] %>/
<% else %>
ProxyPass <%= node['tomcat']['apache_tomcat_bind_path'] %> http://localhost:<%= node['tomcat']['port'] %>/
ProxyPassReverse <%= node['tomcat']['apache_tomcat_bind_path'] %> http://localhost:<%= node['tomcat']['port'] %>/
<% end -%>
```

The template uses the [ProxyPass](https://httpd.apache.org/docs/2.0/mod/mod_proxy.html#proxypass) and [ProxyPassReverse](https://httpd.apache.org/docs/2.0/mod/mod_proxy.html#proxypassreverse) directives to configure the port used to pass traffic between Apache and Tomcat\. Because both servers are on the same instance, they can use a localhost URL and are both set by default to `http://localhost:8080`\.