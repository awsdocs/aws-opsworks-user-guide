# Step 6: Deploy and run the App<a name="other-services-redis-app"></a>

This example assumes that you have Ruby on Rails application that uses Redis\. To access the configuration file, you can add the `redis` gem to your Gemfile and create a Rails initializer in `config/initializers/redis.rb` as follows:

```
REDIS_CONFIG = YAML::load_file(Rails.root.join('config', 'redis.yml'))
$redis = Redis.new(:host => REDIS_CONFIG['host'], :port => REDIS_CONFIG['port'])
```

Then [create an app](workingapps-creating.md) to represent your application and [deploy it](workingapps-deploying.md) to the Rails App Server layer's instances, which updates the application code and runs `generate.rb` to generate the configuration file\. When you run the application, it will use the ElastiCache Redis instance as its in\-memory key\-value store\.