# rails\_stack Attributes<a name="attributes-json-opsworks-rails-stack"></a>

**name**  
Specifies the rails stack, and is set to `"apache_passenger"` or `"nginx_unicorn"` \(string\)\.  

```
node["opsworks"]["rails_stack"]["name"]
```

**recipe **  
The associated recipe, which depends on whether you are using Passenger or Unicorn \(string\):  

+ Unicorn: `"unicorn::rails"`

+ Passenger: `"passenger_apache2::rails"`

```
node["opsworks"]["rails_stack"]["recipe"]
```

**restart\_command **  
The restart command, which depends on whether you are using Passenger or Unicorn \(string\):  

+ Unicorn: `"../../shared/scripts/unicorn clean-restart"`

+ Passenger: `"touch tmp/restart.txt"`

**service **  
The service name, which depends on whether you are using Passenger or Unicorn \(string\):  

+ Unicorn: `"unicorn"`

+ Passenger: `"apache2"`

```
node["opsworks"]["rails_stack"]["service"]
```