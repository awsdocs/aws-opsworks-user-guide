# rails\_stack Attributes<a name="attributes-json-opsworks-rails-stack"></a>

**name**  <a name="attributes-json-opsworks-rails-stack-name"></a>
Specifies the rails stack, and is set to `"apache_passenger"` or `"nginx_unicorn"` \(string\)\.  

```
node["opsworks"]["rails_stack"]["name"]
```

**recipe **  <a name="attributes-json-opsworks-rails-stack-recipe"></a>
The associated recipe, which depends on whether you are using Passenger or Unicorn \(string\):  
+ Unicorn: `"unicorn::rails"`
+ Passenger: `"passenger_apache2::rails"`

```
node["opsworks"]["rails_stack"]["recipe"]
```

**restart\_command **  <a name="attributes-json-opsworks-rails-stack-restart"></a>
The restart command, which depends on whether you are using Passenger or Unicorn \(string\):  
+ Unicorn: `"../../shared/scripts/unicorn clean-restart"`
+ Passenger: `"touch tmp/restart.txt"`

**service **  <a name="attributes-json-opsworks-rails-stack-service"></a>
The service name, which depends on whether you are using Passenger or Unicorn \(string\):  
+ Unicorn: `"unicorn"`
+ Passenger: `"apache2"`

```
node["opsworks"]["rails_stack"]["service"]
```