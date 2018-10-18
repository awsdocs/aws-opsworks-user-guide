# unicorn Attributes<a name="attributes-recipes-unicorn"></a>

**Note**  
These attributes are available only on Linux stacks\.

The [`unicorn` attributes](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/unicorn/attributes/default.rb) specify the [Unicorn](http://unicorn.bogomips.org/) configuration\. For more information, see [Unicorn::Configurator](http://unicorn.bogomips.org/Unicorn/Configurator.html)\. For more information on how to override built\-in attributes to specify custom values, see [Overriding Attributes](workingcookbook-attributes.md)\.


****  

|  |  |  | 
| --- |--- |--- |
| [accept\_filter](#attributes-recipes-unicorn-accept) | [backlog](#attributes-recipes-unicorn-backlog) | [delay](#attributes-recipes-unicorn-delay) | 
| [tcp\_nodelay](#attributes-recipes-unicorn-nodelay) | [tcp\_nopush](#attributes-recipes-unicorn-nopush) | [preload\_app](#attributes-recipes-unicorn-preload) | 
| [timeout](#attributes-recipes-unicorn-timeout) | [tries](#attributes-recipes-unicorn-tries) | [version](#attributes-recipes-unicorn-version) | 
| [worker\_processes](#attributes-recipes-unicorn-worker) |  |  | 

**accept\_filter**  <a name="attributes-recipes-unicorn-accept"></a>
The accept filter, `'httpready'` or `'dataready'` \(string\)\. The default value is `'httpready'`\.  

```
node[:unicorn][:accept_filter]
```

**backlog**  <a name="attributes-recipes-unicorn-backlog"></a>
The maximum number of requests that the queue can hold \(number\)\. The default value is `1024`\.  

```
node[:unicorn][:backlog]
```

**delay**  <a name="attributes-recipes-unicorn-delay"></a>
The amount of time, in seconds, to wait to retry binding a socket \(number\)\. The default value is `0.5`\.  

```
node[:unicorn][:delay]
```

**preload\_app**  <a name="attributes-recipes-unicorn-preload"></a>
Whether to preload an app before forking a worker process \(Boolean\)\. The default value is `true`\.  

```
node[:unicorn][:preload_app]
```

**tcp\_nodelay**  <a name="attributes-recipes-unicorn-nodelay"></a>
Whether to disable Nagle's algorithm for TCP sockets \(Boolean\)\. The default value is `true`\.  

```
node[:unicorn][:tcp_nodelay]
```

**tcp\_nopush**  <a name="attributes-recipes-unicorn-nopush"></a>
Whether to enable TCP\_CORK \(Boolean\)\. The default value is `false`\.  

```
node[:unicorn][:tcp_nopush]
```

**timeout**  <a name="attributes-recipes-unicorn-timeout"></a>
The maximum amount time, in seconds, that a worker is allowed to use for each request \(number\)\. Workers that exceed the timeout value are terminated\. The default value is `60`\.  

```
node[:unicorn][:timeout]
```

**tries**  <a name="attributes-recipes-unicorn-tries"></a>
The maximum number of times to retry binding to a socket \(number\)\. The default value is `5`\.  

```
node[:unicorn][:tries]
```

**version**  <a name="attributes-recipes-unicorn-version"></a>
The Unicorn version \(string\)\. The default value is `'4.7.0'`\.  

```
node[:unicorn][:version]
```

**worker\_processes**  <a name="attributes-recipes-unicorn-worker"></a>
The number of worker processes \(number\)\. The default value is `max_pool_size`, if it exists, and `4` otherwise\.  

```
node[:unicorn][:worker_processes]
```