# passenger\_apache2 Attributes<a name="attributes-recipes-passenger"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`passenger_apache2` attributes](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/passenger_apache2/attributes/passenger.rb) specify the [Phusion Passenger](https://www.phusionpassenger.com/) configuration\. For more information, see [Phusion Passenger users guide, Apache version](http://www.modrails.com/documentation/Users%20guide%20Apache.html)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.


****  

|  |  |  | 
| --- |--- |--- |
| [friendly\_error\_pages](#attributes-recipes-passenger-friendly-error-pages) | [gem\_bin ](#attributes-recipes-passenger-gem-bin) | [gems\_path](#attributes-recipes-passenger-gems-path) | 
| [high\_performance\_mode ](#attributes-recipes-passenger-perf) | [root\_path ](#attributes-recipes-passenger-root) | [max\_instances\_per\_app ](#attributes-recipes-passenger-instances) | 
| [max\_pool\_size ](#attributes-recipes-passenger-max-pool) | [max\_requests](#attributes-recipes-passenger-max-requests) | [module\_path ](#attributes-recipes-passenger-mod_path) | 
| [pool\_idle\_time ](#attributes-recipes-passenger-pool-idle) | [rails\_app\_spawner\_idle\_time ](#attributes-recipes-passenger-rails-app) | [rails\_framework\_spawner\_idle\_time ](#attributes-recipes-passenger-rails-framework) | 
| [rails\_spawn\_method ](#attributes-recipes-passenger-rails-spawn) | [ruby\_bin ](#attributes-recipes-passenger-ruby-bin) | [ruby\_wrapper\_bin ](#attributes-recipes-passenger-ruby-wrapper) | 
| [stat\_throttle\_rate ](#attributes-recipes-passenger-throttle) | [version](#attributes-recipes-passenger-version) |  | 

**friendly\_error\_pages**  <a name="attributes-recipes-passenger-friendly-error-pages"></a>
Whether to display a friendly error page if an application fails to start \(string\)\. This attribute can be set to 'on' or 'off'; the default value is 'off'\.   

```
node[:passenger][:friendly_error_pages]
```

**gem\_bin **  <a name="attributes-recipes-passenger-gem-bin"></a>
The location of the Gem binaries \(string\)\. The default value is `'/usr/local/bin/gem'`\.  

```
node[:passenger][:gem_bin]
```

**gems\_path**  <a name="attributes-recipes-passenger-gems-path"></a>
The gems path \(string\)\. The default value depends on the Ruby version\. For example:  
+ Ruby version 1\.8: `'/usr/local/lib/ruby/gems/1.8/gems'`
+ Ruby version 1\.9: `'/usr/local/lib/ruby/gems/1.9.1/gems'`

```
node[:passenger][:gems_path]
```

**high\_performance\_mode **  <a name="attributes-recipes-passenger-perf"></a>
Whether to use Passenger's high\-performance mode \(string\)\. The possible values are `'on'` and `'off'`\. The default value is `'off'`\.  

```
node[:passenger][:high_performance_mode ]
```

**root\_path **  <a name="attributes-recipes-passenger-root"></a>
The Passenger root directory \(string\)\. The default value depends on the Ruby and Passenger versions\. In Chef syntax, the value is `"#{node[:passenger][:gems_path]}/passenger-#{passenger[:version]}"`\.  

```
node[:passenger][:root_path]
```

**max\_instances\_per\_app **  <a name="attributes-recipes-passenger-instances"></a>
The maximum number of application processes per app \(number\)\. The default value is `0`\. For more information, see [PassengerMaxInstancesPerApp](http://www.modrails.com/documentation/Users%20guide%20Apache.html#_passengermaxinstancesperapp_lt_integer_gt)\.  

```
node[:passenger][:max_instances_per_app]
```

**max\_pool\_size **  <a name="attributes-recipes-passenger-max-pool"></a>
The maximum number of application processors \(number\)\. The default value is `8`\. For more information, see [PassengerMaxPoolSize](http://www.modrails.com/documentation/Users%20guide%20Apache.html#_passengermaxpoolsize_lt_integer_gt)\.  

```
node[:passenger][:max_pool_size]
```

**max\_requests**  <a name="attributes-recipes-passenger-max-requests"></a>
The maximum number of requests \(number\)\. The default value is `0`\.  

```
node[:passenger][:max_requests]
```

**module\_path **  <a name="attributes-recipes-passenger-mod_path"></a>
The module path \(string\)\. The default values are as follows:  
+ Amazon Linux and RHEL: `"#{node['apache']['[libexecdir](attributes-recipes-apache.md#attributes-recipes-apache-libexecdir)']}/mod_passenger.so"`
+ Ubuntu: `"#{passenger[:[root\_path](#attributes-recipes-passenger-root)]}/ext/apache2/mod_passenger.so"`

```
node[:passenger][:module_path]
```

**pool\_idle\_time **  <a name="attributes-recipes-passenger-pool-idle"></a>
The maximum time, in seconds, that an application process can be idle \(number\)\. The default value is `14400` \(4 hours\)\. For more information, see [PassengerPoolIdleTime](http://www.modrails.com/documentation/Users%20guide%20Apache.html#PassengerPoolIdleTime)\.  

```
node[:passenger][:pool_idle_time]
```

**rails\_app\_spawner\_idle\_time **  <a name="attributes-recipes-passenger-rails-app"></a>
The maximum idle time for the Rails app spawner \(number\)\. If this attribute is set to zero, the app spawner does not time out\. The default value is `0`\. For more information, see [Spawning Methods Explained](http://www.modrails.com/documentation/Users%20guide%20Apache.html#spawning_methods_explained)\.  

```
node[:passenger][:rails_app_spawner_idle_time]
```

**rails\_framework\_spawner\_idle\_time **  <a name="attributes-recipes-passenger-rails-framework"></a>
The maximum idle time for the Rails framework spawner \(number\)\. If this attribute is set to zero, the framework spawner does not time out\. The default value is `0`\. For more information, see [Spawning Methods Explained](http://www.modrails.com/documentation/Users%20guide%20Apache.html#spawning_methods_explained)\.  

```
node[:passenger][:rails_framework_spawner_idle_time]
```

**rails\_spawn\_method **  <a name="attributes-recipes-passenger-rails-spawn"></a>
The Rails spawn method \(string\)\. The default value is `'smart-lv2'`\. For more information, see [Spawning Methods Explained](http://www.modrails.com/documentation/Users%20guide%20Apache.html#spawning_methods_explained)\.  

```
node[:passenger][:rails_spawn_method]
```

**ruby\_bin **  <a name="attributes-recipes-passenger-ruby-bin"></a>
The location of the Ruby binaries \(string\)\. The default value is `'/usr/local/bin/ruby'`\.  

```
node[:passenger][:ruby_bin]
```

**ruby\_wrapper\_bin **  <a name="attributes-recipes-passenger-ruby-wrapper"></a>
The location of the Ruby wrapper script \(string\)\. The default value is `'/usr/local/bin/ruby_gc_wrapper.sh'`\.  

```
node[:passenger][:ruby_wrapper_bin]
```

**stat\_throttle\_rate **  <a name="attributes-recipes-passenger-throttle"></a>
The rate at which Passenger performs file system checks \(number\)\. The default value is `5`, which means that the checks will be performed at most once every 5 seconds\. For more information, see [PassengerStatThrottleRate ](http://www.modrails.com/documentation/Users%20guide%20Apache.html#_passengerstatthrottlerate_lt_integer_gt)\.  

```
node[:passenger][:stat_throttle_rate]
```

**version**  <a name="attributes-recipes-passenger-version"></a>
The version \(string\)\. The default value is `'3.0.9'`\.  

```
node[:passenger][:version]
```