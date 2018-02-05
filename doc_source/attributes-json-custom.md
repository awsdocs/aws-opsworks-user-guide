# opsworks\_custom\_cookbooks Attributes<a name="attributes-json-custom"></a>

Contains attributes that specify the stack's custom cookbooks\.

**enabled**  
Whether custom cookbooks are enabled \(Boolean\)\.  

```
node["opsworks_custom_cookbooks"]["enabled"]
```

**recipes**  
A list of the recipes that are to be executed for this command, including custom recipes, using the `cookbookname::recipename` format \(list of string\)\.  

```
node["opsworks_custom_cookbooks"]["recipes"]
```