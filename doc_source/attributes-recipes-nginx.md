# nginx Attributes<a name="attributes-recipes-nginx"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`nginx` attributes](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/nginx/attributes/nginx.rb) specify the [Nginx](http://wiki.nginx.org/Main) configuration\. For more information, see [Directive Index](http://wiki.nginx.org/DirectiveIndex)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.


****  

|  |  |  | 
| --- |--- |--- |
| [binary ](#attributes-recipes-nginx-binary) | [dir ](#attributes-recipes-nginx-dir) | [gzip ](#attributes-recipes-nginx-gzip) | 
| [gzip\_comp\_level ](#attributes-recipes-nginx-gzip-comp) | [gzip\_disable ](#attributes-recipes-nginx-gzip-disable) | [gzip\_http\_version ](#attributes-recipes-nginx-gzip-http) | 
| [gzip\_proxied ](#attributes-recipes-nginx-gzip-proxied) | [gzip\_static ](#attributes-recipes-nginx-gzip-static) | [gzip\_types ](#attributes-recipes-nginx-gzip-types) | 
| [gzip\_vary ](#attributes-recipes-nginx-gzip-vary) | [keepalive ](#attributes-recipes-nginx-keepalive) | [keepalive\_timeout ](#attributes-recipes-nginx-keepalive-timeout) | 
| [log\_dir ](#attributes-recipes-nginx-log) | [user ](#attributes-recipes-nginx-user) | [server\_names\_hash\_bucket\_size](#attributes-recipes-nginx-worker-hash) | 
| [worker\_processes ](#attributes-recipes-nginx-worker-processes) | [worker\_connections ](#attributes-recipes-nginx-worker-connections) |  | 

**binary **  <a name="attributes-recipes-nginx-binary"></a>
The location of the Nginx binaries \(string\)\. The default value is `'/usr/sbin/nginx'`\.  

```
node[:nginx][:binary]
```

**dir **  <a name="attributes-recipes-nginx-dir"></a>
The location of files such as configuration files \(string\)\. The default value is `'/etc/nginx'`\.  

```
node[:nginx][:dir]
```

**gzip **  <a name="attributes-recipes-nginx-gzip"></a>
Whether gzip compression is enabled \(string\)\. The possible values are `'on'` and `'off'`\. The default value is `'on'`\.  
Compression can introduce security risks\. To completely disable compression, set this attribute as follows:  

```
node[:nginx][:gzip] = 'off'
```

```
node[:nginx][:gzip]
```

**gzip\_comp\_level **  <a name="attributes-recipes-nginx-gzip-comp"></a>
The compression level, which can range from 1–9, with 1 corresponding to the least compression \(string\)\. The default value is `'2'`\.  

```
node[:nginx][:gzip_comp_level]
```

**gzip\_disable **  <a name="attributes-recipes-nginx-gzip-disable"></a>
Disables gzip compression for specified user agents \(string\)\. The value is a regular expression and the default value is `'MSIE [1-6].(?!.*SV1)'`\.  

```
node[:nginx][:gzip_disable]
```

**gzip\_http\_version **  <a name="attributes-recipes-nginx-gzip-http"></a>
Enables gzip compression for a specified HTTP version \(string\)\. The default value is `'1.0'`\.  

```
node[:nginx][:gzip_http_version]
```

**gzip\_proxied **  <a name="attributes-recipes-nginx-gzip-proxied"></a>
Whether and how to compress the response to proxy requests, which can take one of the following values \(string\):  
+ `'off'`: do not compress proxied requests
+ `'expired'`: compress if the Expire header prevents caching
+ `'no-cache'`: compress if the Cache\-Control header is set to "no\-cache"
+ `'no-store'`: compress if the Cache\-Control header is set to "no\-store"
+ `'private'`: compress if the Cache\-Control header is set to "private"
+ `'no_last_modified'`: compress if Last\-Modified is not set
+ `'no_etag'`: compress if the request lacks an ETag header
+ `'auth'`: compress if the request includes an Authorization header
+ `'any'`: compress all proxied requests 
The default value is `'any'`\.  

```
node[:nginx][:gzip_proxied]
```

**gzip\_static **  <a name="attributes-recipes-nginx-gzip-static"></a>
Whether the gzip static module is enabled \(string\)\. The possible values are `'on'` and `'off'`\. The default value is `'on'`\.  

```
node[:nginx][:gzip_static]
```

**gzip\_types **  <a name="attributes-recipes-nginx-gzip-types"></a>
A list of MIME types to be compressed \(list of string\)\. The default value is `['text/plain', 'text/html', 'text/css', 'application/x-javascript', 'text/xml', 'application/xml', 'application/xml+rss', 'text/javascript']`\.  

```
node[:nginx][:gzip_types]
```

**gzip\_vary **  <a name="attributes-recipes-nginx-gzip-vary"></a>
Whether to enable a `Vary:Accept-Encoding `response header \(string\)\. The possible values are `'on'` and `'off'`\. The default value is `'on'`\.  

```
node[:nginx][:gzip_vary]
```

**keepalive **  <a name="attributes-recipes-nginx-keepalive"></a>
Whether to enable a keep\-alive connection \(string\)\. The possible values are `'on'` and `'off'`\. The default value is `'on'`\.  

```
node[:nginx][:keepalive]
```

**keepalive\_timeout **  <a name="attributes-recipes-nginx-keepalive-timeout"></a>
The maximum amount of time, in seconds, that a keep\-alive connection remains open \(number\)\. The default value is `65`\.  

```
node[:nginx][:keepalive_timeout]
```

**log\_dir **  <a name="attributes-recipes-nginx-log"></a>
The location of the log files \(string\)\. The default value is `'/var/log/nginx'`\.  

```
node[:nginx][:log_dir]
```

**user **  <a name="attributes-recipes-nginx-user"></a>
The user \(string\)\. The default values are as follows:  
+ Amazon Linux and RHEL: `'www-data'`
+ Ubuntu: `'nginx'`

```
node[:nginx][:user]
```

**server\_names\_hash\_bucket\_size**  <a name="attributes-recipes-nginx-worker-hash"></a>
The bucket size for hash tables of server names, which can be set to `32`, `64`, or `128` \(number\)\. The default value is `64`\.  

```
node[:nginx][:server_names_hash_bucket_size]
```

**worker\_processes **  <a name="attributes-recipes-nginx-worker-processes"></a>
The number of worker processes \(number\)\. The default value is `10`\.  

```
node[:nginx][:worker_processes]
```

**worker\_connections **  <a name="attributes-recipes-nginx-worker-connections"></a>
The maximum number of worker connections \(number\)\. The default value is `1024`\. The maximum number of clients is set to `worker_processes * worker_connections`\.  

```
node[:nginx][:worker_connections]
```