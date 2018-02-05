# mysql Attributes<a name="attributes-json-mysql"></a>

Contains a set of attributes that specify the MySQL database server configuration\.

**clients**  
A list of client IP addresses \(list of string\)\.  

```
node["mysql"]["clients"]
```

**server\_root\_password**  
The root password \(string\)\.  

```
node["mysql"]["server_root_password"]
```