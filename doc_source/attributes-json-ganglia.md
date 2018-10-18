# ganglia Attributes<a name="attributes-json-ganglia"></a>

Contains a **web** attribute that contains several attributes that specify how to access the Ganglia statistics web page:

**password**  <a name="attributes-json-ganglia-password"></a>
The password required to access the statistics page \(string\)\.  

```
node["ganglia"]["web"]["password"]
```

**url**  <a name="attributes-json-ganglia-url"></a>
The statistics page's URL path, such as `"/ganglia"` \(string\)\. The complete URL is `http://DNSNameURLPath`, where `DNSName` is the associated instance's DNS name\.  

```
node["ganglia"]["web"]["url"]
```

**user**  <a name="attributes-json-ganglia-user"></a>
The user name required to access the statistics page \(string\)\.  

```
node["ganglia"]["web"]["user"]
```