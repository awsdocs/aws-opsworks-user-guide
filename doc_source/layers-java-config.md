# Custom Configuration<a name="layers-java-config"></a>

AWS OpsWorks Stacks exposes additional configuration settings as built\-in attributes, which are all in the `opsworks_java` namespace\. You can use custom JSON or a custom attributes file to override the built\-in attributes and specify custom values\. For example, the JVM and Tomcat versions are represented by the built\-in `jvm_version` and `java_app_server_version` attributes, both of which are set to 7\. You can use custom JSON or a custom attributes file to set either or both to 6\. The following example uses custom JSON to set both attributes to 6:

```
{
  "opsworks_java": {
    "jvm_version": 6,
    "java_app_server_version" : 6
  }
}
```

For more information, see [Using Custom JSON](workingstacks-json.md)\.

Another example of custom configuration is installing a custom JDK by overriding the `use_custom_pkg_location`, `custom_pkg_location_url_debian`, and `custom_pkg_location_url_rhel` attributes\. 

**Note**  
If you override the built\-in cookbooks, you will need to update those components yourself\. 

For more information on attributes and how to override them, see [Overriding Attributes](workingcookbook-attributes.md)\. For a list of built in attributes, see [opsworks\_java Attributes](attributes-recipes-java.md)\.