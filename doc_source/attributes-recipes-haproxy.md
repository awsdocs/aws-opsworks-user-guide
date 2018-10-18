# haproxy Attributes<a name="attributes-recipes-haproxy"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`haproxy` attributes ](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/haproxy/attributes/default.rb)specify the [HAProxy server](http://haproxy.1wt.eu/) configuration\. For more information, see [HAProxy Docs](http://cbonte.github.io/haproxy-dconv/configuration-1.5.html)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.


****  

|  |  |  | 
| --- |--- |--- |
| [balance ](#attributes-recipes-haproxy-balance) | [check\_interval ](#attributes-recipes-haproxy-interval) | [client\_timeout ](#attributes-recipes-haproxy-client-timeout) | 
| [connect\_timeout ](#attributes-recipes-haproxy-connect-timeout) | [default\_max\_connections ](#attributes-recipes-haproxy-default-max) | [global\_max\_connections ](#attributes-recipes-haproxy-global-max) | 
| [health\_check\_method ](#attributes-recipes-haproxy-health-method) | [health\_check\_url ](#attributes-recipes-haproxy-health-url) | [queue\_timeout ](#attributes-recipes-haproxy-queue-timeout) | 
| [http\_request\_timeout ](#attributes-recipes-haproxy-http-timeout) | [maxcon\_factor\_nodejs\_app ](#attributes-recipes-haproxy-nodejs-app) | [maxcon\_factor\_nodejs\_app\_ssl ](#attributes-recipes-haproxy-nodejs-ssl) | 
| [maxcon\_factor\_php\_app ](#attributes-recipes-haproxy-php-app) | [maxcon\_factor\_php\_app\_ssl ](#attributes-recipes-haproxy-php-ssl) | [maxcon\_factor\_rails\_app ](#attributes-recipes-haproxy-rails-app) | 
| [maxcon\_factor\_rails\_app\_ssl ](#attributes-recipes-haproxy-rails-ssl) | [maxcon\_factor\_static ](#attributes-recipes-haproxy-static-app) | [maxcon\_factor\_static\_ssl ](#attributes-recipes-haproxy-static-ssl) | 
| [retries ](#attributes-recipes-haproxy-retries) | [server\_timeout ](#attributes-recipes-haproxy-server-timeout) | [stats\_url ](#attributes-recipes-haproxy-stats-url) | 
| [stats\_user ](#attributes-recipes-haproxy-user) |  |  | 

**balance **  <a name="attributes-recipes-haproxy-balance"></a>
The algorithm used by a load balancer to select a server \(string\)\. The default value is `'roundrobin'`\. The other options are:  
+ 'static\-rr'
+ 'leastconn'
+ 'source'
+ 'uri'
+ 'url\_param'
+ 'hdr\(name\)'
+ 'rdp\-cookie'
+ 'rdp\-cookie\(name\)'
For more information on these arguments, see [balance](http://cbonte.github.io/haproxy-dconv/configuration-1.5.html)\.  

```
node[:haproxy][:balance]
```

**check\_interval **  <a name="attributes-recipes-haproxy-interval"></a>
The health check time interval \(string\)\. The default value is `'10s'`\.  

```
node[:haproxy][:check_interval]
```

**client\_timeout **  <a name="attributes-recipes-haproxy-client-timeout"></a>
The maximum amount of time that a client can be inactive \(string\)\. The default value is `'60s'`\.  

```
node[:haproxy][:client_timeout]
```

**connect\_timeout **  <a name="attributes-recipes-haproxy-connect-timeout"></a>
The maximum amount of time that HAProxy will wait for a server connection attempt to succeed \(string\)\. The default value is `'10s'`\.  

```
node[:haproxy][:connect_timeout]
```

**default\_max\_connections **  <a name="attributes-recipes-haproxy-default-max"></a>
The default maximum number of connections \(string\)\. The default value is `'80000'`\.  

```
node[:haproxy][:default_max_connections]
```

**global\_max\_connections **  <a name="attributes-recipes-haproxy-global-max"></a>
The maximum number of connections \(string\)\. The default value is `'80000'`\.  

```
node[:haproxy][:global_max_connections]
```

**health\_check\_method **  <a name="attributes-recipes-haproxy-health-method"></a>
The health check method \(string\)\. The default value is `'OPTIONS'`\.  

```
node[:haproxy][:health_check_method]
```

**health\_check\_url **  <a name="attributes-recipes-haproxy-health-url"></a>
The URL path that is used to check servers' health \(string\)\. The default value is `'/'`\.  

```
node[:haproxy][:health_check_url ]
```

**queue\_timeout **  <a name="attributes-recipes-haproxy-queue-timeout"></a>
The maximum wait time for a free connection \(string\)\. The default value is `'120s'`\.  

```
node[:haproxy][:queue_timeout]
```

**http\_request\_timeout **  <a name="attributes-recipes-haproxy-http-timeout"></a>
The maximum amount of time that HAProxy will wait for a complete HTTP request \(string\)\. The default value is `'30s'`\.  

```
node[:haproxy][:http_request_timeout]
```

**retries **  <a name="attributes-recipes-haproxy-retries"></a>
The number of retries after server connection failure \(string\)\. The default value is `'3'`\.  

```
node[:haproxy][:retries]
```

**server\_timeout **  <a name="attributes-recipes-haproxy-server-timeout"></a>
The maximum amount of time that a client can be inactive \(string\)\. The default value is `'60s'`\.  

```
node[:haproxy][:server_timeout]
```

**stats\_url **  <a name="attributes-recipes-haproxy-stats-url"></a>
The URL path for the statistics page \(string\)\. The default value is `'/haproxy?stats'`\.  

```
node[:haproxy][:stats_url]
```

**stats\_user **  <a name="attributes-recipes-haproxy-user"></a>
The statistics page user name \(string\)\. The default value is `'opsworks'`\.  

```
node[:haproxy][:stats_user]
```

The `maxcon` attributes represent a load factor multiplier that is used to compute the maximum number of connections that HAProxy allows for [backends](attributes-json-opsworks-instance.md#attributes-json-opsworks-instance-backends)\. For example, suppose you have a Rails app server on a small instance with a `backend` value of 4, which means that AWS OpsWorks Stacks will configure four Rails processes for that instance\. If you use the default `maxcon_factor_rails_app` value of 7, HAProxy will handle 28 \(4\*7\) connections to the Rails server\.

**maxcon\_factor\_nodejs\_app **  <a name="attributes-recipes-haproxy-nodejs-app"></a>
The maxcon factor for a Node\.js app server \(number\)\. The default value is `10`\.  

```
node[:haproxy][:maxcon_factor_nodejs_app]
```

**maxcon\_factor\_nodejs\_app\_ssl **  <a name="attributes-recipes-haproxy-nodejs-ssl"></a>
The maxcon factor for a Node\.js app server with SSL \(number\)\. The default value is `10`\.  

```
node[:haproxy][:maxcon_factor_nodejs_app_ssl]
```

**maxcon\_factor\_php\_app **  <a name="attributes-recipes-haproxy-php-app"></a>
The maxcon factor for a PHP app server \(number\)\. The default value is `10`\.  

```
node[:haproxy][:maxcon_factor_php_app]
```

**maxcon\_factor\_php\_app\_ssl **  <a name="attributes-recipes-haproxy-php-ssl"></a>
The maxcon factor for a PHP app server with SSL \(number\)\. The default value is `10`\.  

```
node[:haproxy][:maxcon_factor_php_app_ssl]
```

**maxcon\_factor\_rails\_app **  <a name="attributes-recipes-haproxy-rails-app"></a>
The maxcon factor for a Rails app server \(number\)\. The default value is `7`\.  

```
node[:haproxy][:maxcon_factor_rails_app]
```

**maxcon\_factor\_rails\_app\_ssl **  <a name="attributes-recipes-haproxy-rails-ssl"></a>
The maxcon factor for a Rails app server with SSL \(number\)\. The default value is `7`\.  

```
node[:haproxy][:maxcon_factor_rails_app_ssl]
```

**maxcon\_factor\_static **  <a name="attributes-recipes-haproxy-static-app"></a>
The maxcon factor for a static web server \(number\)\. The default value is `15`\.  

```
node[:haproxy][:maxcon_factor_static]
```

**maxcon\_factor\_static\_ssl **  <a name="attributes-recipes-haproxy-static-ssl"></a>
The maxcon factor for a static web server with SSL \(number\)\. The default value is `15`\.  

```
node[:haproxy][:maxcon_factor_static_ssl]
```