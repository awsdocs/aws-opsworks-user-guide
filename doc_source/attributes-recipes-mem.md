# memached Attributes<a name="attributes-recipes-mem"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`memached` attributes](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/memcached/attributes/default.rb) specify the [Memcached](http://memcached.org/) server configuration\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.


****  

|  |  |  | 
| --- |--- |--- |
| [memory ](#attributes-recipes-mem-memory) | [max\_connections ](#attributes-recipes-mem-max) | [pid\_file ](#attributes-recipes-mem-pid) | 
| [port ](#attributes-recipes-mem-port) | [start\_command ](#attributes-recipes-mem-start) | [stop\_command ](#attributes-recipes-mem-stop) | 
| [user ](#attributes-recipes-mem-user) |  |  | 

**memory **  
The maximum memory to use, in MB \(number\)\. The default value is `512`\.  

```
node[:memcached][:memory]
```

**max\_connections **  
The maximum number of connections \(string\)\. The default value is `'4096'`\.  

```
node[:memcached][:max_connections]
```

**pid\_file **  
The file that contains the daemon's process ID \(string\)\. The default value is `'var/run/memcached.pid'`\.  

```
node[:memcached][:pid_file]
```

**port **  
The port to listen on \(number\)\. The default value is `11211`\.  

```
node[:memcached][:port]
```

**start\_command **  
The start command \(string\)\. The default value is `'/etc/init.d/memcached start'`\.  

```
node[:memcached][:start_command]
```

**stop\_command **  
The stop command \(string\)\. The default value is `'/etc/init.d/memcached stop'`\.  

```
node[:memcached][:stop_command]
```

**user **  
The user \(string\)\. The default value is `'nobody'`\.  

```
node[:memcached][:user]
```