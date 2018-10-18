# opsworks\_java Attributes<a name="attributes-recipes-java"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`opsworks_java` attributes](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/opsworks_java/attributes/default.rb) specify the [Tomcat](http://tomcat.apache.org/) server configuration\. For more information, see [Apache Tomcat Configuration Reference](http://tomcat.apache.org/tomcat-5.5-doc/config/)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.


****  

|  |  |  | 
| --- |--- |--- |
| [datasources ](#attributes-recipes-java-datasources) | [java\_app\_server\_version ](#attributes-recipes-java-server-version) | [java\_shared\_lib\_dir ](#attributes-recipes-java-shared-lib) | 
| [jvm\_pkg Attributes ](#attributes-recipes-java-pkg) | [custom\_pkg\_location\_url\_debian ](#attributes-recipes-java-pkg-debian) | [java\_home\_basedir ](#attributes-recipes-java-pkg-basedir) | 
| [custom\_pkg\_location\_url\_rhel ](#attributes-recipes-java-pkg-rhel) | [use\_custom\_pkg\_location ](#attributes-recipes-java-pkg-use) | [jvm\_options ](#attributes-recipes-java-jvm-options) | 
| [jvm\_version ](#attributes-recipes-java-jvm-version) | [tomcat Attributes](#attributes-recipes-java-tomcat) |  | 

**datasources **  <a name="attributes-recipes-java-datasources"></a>
A set of attributes that define JNDI resource names \(string\)\. For more information on how to use this attribute, see [Deploying a JSP App with a Back\-End Database](layers-java-deploy.md#layers-java-deploy-jsp-db)\. The default value is an empty hash, which can be filled with custom mappings between app short names and JNDI names\. For more information, see [Deploying a JSP App with a Back\-End Database](layers-java-deploy.md#layers-java-deploy-jsp-db)\.  

```
node['opsworks_java']['datasources']
```

**java\_app\_server\_version **  <a name="attributes-recipes-java-server-version"></a>
The Java app server version \(number\)\. The default value is `7`\. You can override this attribute to specify version 6\. If you install a nondefault JDK, this attribute is ignored\.  

```
node['opsworks_java']['java_app_server_version']
```

**java\_shared\_lib\_dir **  <a name="attributes-recipes-java-shared-lib"></a>
The directory for the Java shared libraries \(string\)\. The default value is `/usr/share/java`\.  

```
node['opsworks_java']['java_shared_lib_dir']
```

**jvm\_pkg Attributes **  <a name="attributes-recipes-java-pkg"></a>
A set of attributes that you can override to install a nondefault JDK\.    
**use\_custom\_pkg\_location **  <a name="attributes-recipes-java-pkg-use"></a>
Whether to install a custom JDK instead of OpenJDK \(Boolean\)\. The default value is `false`\.  

```
node['opsworks_java']['jvm_pkg']['use_custom_pkg_location']
```  
**custom\_pkg\_location\_url\_debian **  <a name="attributes-recipes-java-pkg-debian"></a>
The location of the JDK package to be installed on Ubuntu instances \(string\)\. The default value is `'http://aws.amazon.com/'`, which is simply an initialization value with no proper meaning\. If you want to install a nondefault JDK, you must override this attribute and set it to the appropriate URL\.  

```
node['opsworks_java']['jvm_pkg']['custom_pkg_location_url_debian']
```  
**custom\_pkg\_location\_url\_rhel **  <a name="attributes-recipes-java-pkg-rhel"></a>
The location of the JDK package to be installed on Amazon Linux and RHEL instances \(string\)\. The default value is `'http://aws.amazon.com/'`, which is simply an initialization value with no proper meaning\. If you want to install a nondefault JDK, you must override this attribute and set it to the appropriate URL\.  

```
node['opsworks_java']['jvm_pkg']['custom_pkg_location_url_rhel']
```  
**java\_home\_basedir **  <a name="attributes-recipes-java-pkg-basedir"></a>
The directory that the JDK package will be extracted to \(string\)\. The default value is `/usr/local`\. You do not need to specify this setting for RPM packages; they include a complete directory structure\.  

```
node['opsworks_java']['jvm_pkg']['java_home_basedir']
```

**jvm\_options **  <a name="attributes-recipes-java-jvm-options"></a>
The JVM command line options, which allow you to specify settings such as the heap size \(string\)\. A common set of options is `-Djava.awt.headless=true -Xmx128m -XX:+UseConcMarkSweepGC`\. The default value is no options\.  

```
node['opsworks_java']['jvm_options']
```

**jvm\_version **  <a name="attributes-recipes-java-jvm-version"></a>
The OpenJDK version \(number\)\. The default value is `7`\. You can override this attribute to specify OpenJDK version 6\. If you install a nondefault JDK, this attribute is ignored\.  

```
node['opsworks_java']['jvm_version']
```

**tomcat Attributes**  <a name="attributes-recipes-java-tomcat"></a>
A set of attributes that you can override to install the default Tomcat configuration\.    
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/attributes-recipes-java.html)  
**ajp\_port **  <a name="attributes-recipes-java-ajp-port"></a>
The AJP port \(number\)\. The default value is `8009`\.  

```
node['opsworks_java']['tomcat]['ajp_port']
```  
**apache\_tomcat\_bind\_mod **  <a name="attributes-recipes-java-bind-mod"></a>
The proxy module \(string\)\. The default value is `proxy_http`\. You can override this attribute to specify the AJP proxy module, `proxy_ajp`\.  

```
node['opsworks_java']['tomcat]['apache_tomcat_bind_mod']
```  
**apache\_tomcat\_bind\_path **  <a name="attributes-recipes-java-bind-path"></a>
The Apache\-Tomcat bind path \(string\)\. The default value is `/`\. You should not override this attribute; changing the bind path can cause the application to stop working\.  

```
node['opsworks_java']['tomcat]['apache_tomcat_bind_path']
```  
**auto\_deploy **  <a name="attributes-recipes-java-deploy"></a>
Whether to autodeploy \(Boolean\)\. The default value is `true`\.  

```
node['opsworks_java']['tomcat]['auto_deploy']
```  
**connection\_timeout **  <a name="attributes-recipes-java-timeout"></a>
The connection timeout, in milliseconds \(number\)\. The default value is `20000` \(20 seconds\)\.  

```
node['opsworks_java']['tomcat]['connection_timeout']
```  
**mysql\_connector\_jar **  <a name="attributes-recipes-java-connector"></a>
The MySQL connector library's JAR file \(string\)\. The default value is `mysql-connector-java.jar`\.  

```
node['opsworks_java']['tomcat]['mysql_connector_jar']
```  
**port **  <a name="attributes-recipes-java-port"></a>
The standard port \(number\)\. The default value is `8080`\.  

```
node['opsworks_java']['tomcat]['port']
```  
**secure\_port **  <a name="attributes-recipes-java-secure-port"></a>
The secure port \(number\)\. The default value is `8443`\.  

```
node['opsworks_java']['tomcat]['secure_port']
```  
**shutdown\_port **  <a name="attributes-recipes-java-shutdown-port"></a>
 The shutdown port \(number\)\. The default value is `8005`\.  

```
node['opsworks_java']['tomcat]['shutdown_port']
```  
**threadpool\_max\_threads **  <a name="attributes-recipes-java-threadpool-max"></a>
The maximum number of threads in the thread pool \(number\)\. The default value is `150`\.  

```
node['opsworks_java']['tomcat]['threadpool_max_threads']
```  
**threadpool\_min\_spare\_threads **  <a name="attributes-recipes-java-threadpool-min"></a>
The minimum number of spare threads in the thread pool \(number\)\. The default value is `4`\.  

```
node['opsworks_java']['tomcat]['threadpool_min_spare_threads']
```  
**unpack\_wars **  <a name="attributes-recipes-java-unpack"></a>
Whether to unpack WAR files \(Boolean\)\. The default value is `true`\.  

```
node['opsworks_java']['tomcat]['unpack_wars']
```  
**uri\_encoding **  <a name="attributes-recipes-java-encoding"></a>
The URI encoding \(string\)\. The default value is `UTF-8`\.  

```
node['opsworks_java']['tomcat]['uri_encoding']
```  
**use\_ssl\_connector **  <a name="attributes-recipes-java-ssl"></a>
Whether to use an SSL connector \(Boolean\)\. The default value is `false`\.  

```
node['opsworks_java']['tomcat]['use_ssl_connector']
```  
**use\_threadpool **  <a name="attributes-recipes-java-threadpool"></a>
Whether to use a thread pool \(Boolean\)\. The default value is `false`\.  

```
node['opsworks_java']['tomcat]['use_threadpool']
```  
**userdatabase\_pathname **  <a name="attributes-recipes-java-userdb"></a>
The user database path name \(string\)\. The default value is `conf/tomcat-users.xml`\.  

```
node['opsworks_java']['tomcat]['userdatabase_pathname']
```