# mysql Attributes<a name="attributes-recipes-mysql"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`mysql` attributes](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/mysql/attributes/server.rb) specify the [MySQL](http://www.mysql.com/) master configuration\. For more information, see [Server System Variables](http://dev.mysql.com/doc/refman/5.1/en/server-system-variables.html)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.


****  

|  |  |  | 
| --- |--- |--- |
| [basedir ](#attributes-recipes-mysql-basedir) | [bind\_address ](#attributes-recipes-mysql-bind) | [clients ](#attributes-recipes-mysql-clients) | 
| [conf\_dir ](#attributes-recipes-mysql-conf) | [confd\_dir ](#attributes-recipes-mysql-confd) | [datadir ](#attributes-recipes-mysql-datadir) | 
| [grants\_path ](#attributes-recipes-mysql-grants) | [mysql\_bin ](#attributes-recipes-mysql-bin) | [mysqladmin\_bin ](#attributes-recipes-mysql-admin-bin) | 
| [pid\_file ](#attributes-recipes-mysql-pid) | [port ](#attributes-recipes-mysql-port) | [root\_group ](#attributes-recipes-mysql-group) | 
| [server\_root\_password ](#attributes-recipes-mysql-pwd) | [socket ](#attributes-recipes-mysql-socket) | [tunable Attributes](#attributes-recipes-mysql-tunable) | 

**basedir **  
The base directory \(string\)\. The default value is `'/usr'`\.  

```
node[:mysql][:basedir]
```

**bind\_address **  
The address that MySQL listens on \(string\)\. The default value is `'0.0.0.0'`\.  

```
node[:mysql][:bind_address]
```

**clients **  
A list of clients \(list of string\)\.  

```
node[:mysql][:clients]
```

**conf\_dir **  
The directory that contains the configuration file \(string\)\. The default values are as follows:  

+ Amazon Linux and RHEL: `'/etc'`

+ Ubuntu: `'/etc/mysql'`

```
node[:mysql][:conf_dir]
```

**confd\_dir **  
The directory that contains additional configuration files \(string\)\. The default value is `'/etc/mysql/conf.d'`\.  

```
node[:mysql][:confd_dir]
```

**datadir **  
The data directory \(string\)\. The default value is `'/var/lib/mysql'`\.  

```
node[:mysql][:datadir]
```

**grants\_path **  
The grant table location \(string\)\. The default value is `'/etc/mysql_grants.sql'`\.  

```
node[:mysql][:grants_path]
```

**mysql\_bin **  
The mysql binaries location \(string\)\. The default value is `'/usr/bin/mysql'`\.  

```
node[:mysql][:mysql_bin]
```

**mysqladmin\_bin **  
The mysqladmin location \(string\)\. The default value is `'/usr/bin/mysqladmin'`\.  

```
node[:mysql][:mysqladmin_bin]
```

**pid\_file **  
The file that contains the daemon's process ID \(string\)\. The default value is `'/var/run/mysqld/mysqld.pid'`\.  

```
node[:mysql][:pid_file]
```

**port **  
The port that the server listens on \(number\)\. The default value is `3306`\.  

```
node[:mysql][:port]
```

**root\_group **  
The root group \(string\)\. The default value is `'root'`\.  

```
node[:mysql][:root_group]
```

**server\_root\_password **  
The server's root password \(string\)\. The default value is randomly generated\.  

```
node[:mysql][:server_root_password]
```

**socket **  
The location of the socket file \(string\)\. The default value is `'/var/lib/mysql/mysql.sock'`\. The default values are as follows:  

+ Amazon Linux and RHEL: `'/var/lib/mysql/mysql.sock'`

+ Ubuntu: `'/var/run/mysqld/mysqld.sock'`

```
node[:mysql][:socket]
```

**tunable Attributes**  
The tunable attributes are used for performance tuning\.    
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/attributes-recipes-mysql.html)  
**back\_log **  
The maximum number of outstanding requests \(string\)\. The default value is `'128'`\.  

```
node[:mysql][:tunable][:back_log]
```  
**innodb\_additional\_mem\_pool\_size **  
The size of the pool that [Innodb](http://dev.mysql.com/doc/refman/5.5/en/innodb-storage-engine.html) uses to store internal data structures \(string\)\. The default value is `'20M'`\.  

```
node[:mysql][:tunable][:innodb_additional_mem_pool_size]
```  
**innodb\_buffer\_pool\_size **  
The [Innodb](http://dev.mysql.com/doc/refman/5.5/en/innodb-storage-engine.html) buffer pool size \(string\)\. The attribute value is set by AWS OpsWorks Stacks and depends on the instance type, but you can override it by using custom JSON or a custom attribute file\.   

```
node[:mysql][:tunable][:innodb_buffer_pool_size]
```  
**innodb\_flush\_log\_at\_trx\_commit **  
How often [Innodb](http://dev.mysql.com/doc/refman/5.5/en/innodb-storage-engine.html) flushes the log buffer \(string\)\. The default value is `'2'`\. For more information, see [innodb\_flush\_log\_at\_trx\_commit](http://dev.mysql.com/doc/refman/5.1/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit)\.  

```
node[:mysql][:tunable][:innodb_flush_log_at_trx_commit]
```  
**innodb\_lock\_wait\_timeout **  
The maximum amount of time, in seconds, that an [Innodb](http://dev.mysql.com/doc/refman/5.5/en/innodb-storage-engine.html) transaction waits for a row lock \(string\)\. The default value is `'50'`\.  

```
node[:mysql][:tunable][:innodb_lock_wait_timeout]
```  
**key\_buffer **  
The index buffer size \(string\)\. The default value is `'250M'`\.  

```
node[:mysql][:tunable][:key_buffer]
```  
**log\_slow\_queries **  
The location of the slow\-query log file \(string\)\. The default value is `'/var/log/mysql/mysql-slow.log'`\.  

```
node[:mysql][:tunable][:log_slow_queries]
```  
**long\_query\_time **  
The time, in seconds, required to designate a query as a long query \(string\)\. The default value is `'1'`\.  

```
node[:mysql][:tunable][:long_query_time]
```  
**max\_allowed\_packet **  
The maximum allowed packet size \(string\)\. The default value is `'32M'`\.  

```
node[:mysql][:tunable][:max_allowed_packet]
```  
**max\_connections **  
The maximum number of concurrent client connections \(string\)\. The default value is `'2048'`\.  

```
node[:mysql][:tunable][:max_connections]
```  
**max\_heap\_table\_size **  
The maximum size of user\-created `MEMORY` tables \(string\)\. The default value is `'32M'`\.  

```
node[:mysql][:tunable][:max_heap_table_size]
```  
**net\_read\_timeout **  
The amount of time, in seconds, to wait for more data from a connection \(string\)\. The default value is `'30'`\.  

```
node[:mysql][:tunable][:net_read_timeout]
```  
**net\_write\_timeout **  
The amount of time, in seconds, to wait for a block to be written to a connection \(string\)\. The default value is `'30'`\.  

```
node[:mysql][:tunable][:net_write_timeout]
```  
**query\_cache\_limit **  
The maximum size of an individual cached query \(string\)\. The default value is `'2M'`\.  

```
node[:mysql][:tunable][:query_cache_limit]
```  
**query\_cache\_size **  
The query cache size \(string\)\. The default value is `'128M'`\.  

```
node[:mysql][:tunable][:query_cache_size]
```  
**query\_cache\_type **  
The query cache type \(string\)\. The possible values are as follows:  

+ `'0'`: No caching or retrieval of cached data\.

+ `'1'`: Cache statements that don't begin with `SELECT SQL_NO_CACHE`\. 

+ `'2'`: Cache statements that begin with `SELECT SQL_CACHE`\. 
The default value is `'1'`\.  

```
node[:mysql][:tunable][:query_cache_type]
```  
**thread\_cache\_size **  
The number of client threads that are cached for re\-use \(string\)\. The default value is `'8'`\.  

```
node[:mysql][:tunable][:thread_cache_size]
```  
**thread\_stack **  
The stack size for each thread \(string\)\. The default value is `'192K'`\.  

```
node[:mysql][:tunable][:thread_stack]
```  
**wait\_timeout **  
The amount of time, in seconds, to wait on a noninteractive connection\. The default value is `'180'` \(string\)\.  

```
node[:mysql][:tunable][:wait_timeout]
```  
**table\_cache **  
The number of open tables \(string\)\. The default value is `'2048'`\.  

```
node[:mysql][:tunable][:table_cache]
```