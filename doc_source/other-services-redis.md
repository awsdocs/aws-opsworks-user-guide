# Using ElastiCache Redis as an In\-Memory Key\-Value Store<a name="other-services-redis"></a>

**Note**  
This topic is based on a Linux stack, but Windows stacks can also use Amazon ElastiCache \(ElastiCache\)\. For an example of how of how to use ElastiCache with a Windows instance, see [ElastiCache as an ASP\.NET Session Store](https://aws.amazon.com/blogs/developer/elasticache-as-an-asp-net-session-store/)\.

You can often improve application server performance by using a caching server to provide an in\-memory key\-value store for small items of data such as strings\. Amazon ElastiCache is an AWS service that makes it easy to provide caching support for your application server, using either the [Memcached](http://memcached.org/) or [Redis](https://redis.io) caching engines\. AWS OpsWorks Stacks provides built\-in support for [Memcached](workinglayers-mem.md)\. However, if Redis better suits your requirements, you can customize your stack so that your application servers use ElastiCache Redis\.

This topic walks you through basic process of providing ElastiCache Redis caching support for Linux stacks, using a Rails application server as an example\. It assumes that you already have an appropriate Ruby on Rails application\. For more information on ElastiCache, see [What Is Amazon ElastiCache?](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/WhatIs.html)\.

**Topics**
+ [Step 1: Create an ElastiCache Redis Cluster](other-services-redis-cluster.md)
+ [Step 2: Set up a Rails Stack](other-services-redis-stack.md)
+ [Step 3: Create and Deploy a Custom Cookbook](other-services-redis-cookbook.md)
+ [Step 4: Assign the Recipe to a LifeCycle Event](other-services-redis-event.md)
+ [Step 5: Add Access Information to the Stack Configuration JSON](other-services-redis-json.md)
+ [Step 6: Deploy and run the App](other-services-redis-app.md)