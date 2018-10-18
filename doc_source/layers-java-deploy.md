# Deploying Java Apps<a name="layers-java-deploy"></a>

The following topics describe how to deploy apps to a Java App Server layer's instances\. The examples are for JSP apps, but you can use essentially the same procedure for installing other types of Java app\.

You can deploy JSP pages from any of the supported repositories\. If you want to deploy WAR files, note that AWS OpsWorks Stacks automatically extracts WAR files that are deployed from an Amazon S3 or HTTP archive, but not from a Git or Subversion repository\. If you want to use Git or Subversion for WAR files, you can do one of the following:
+ Store the extracted archive in the repository\.
+ Store the WAR file in the repository and use a Chef deployment hook to extract the archive, as described in the following example\.

You can use Chef deployment hooks to run user\-supplied Ruby applications on an instance at any of four deployment stages\. The application name determines the stage\. The following is an example of a Ruby application named `before_migrate.rb`, which extracts a WAR file that has been deployed from a Git or Subversion repository\. The name associates the application with the Checkout deployment hook so it runs at the beginning of the deployment operation, after the code is checked but before migration\. For more information on how to use this example, see [Using Chef Deployment Hooks](workingcookbook-extend-hooks.md)\.

```
::Dir.glob(::File.join(release_path, '*.war')) do |archive_file|
  execute "unzip_#{archive_file}" do
    command "unzip #{archive_file}"
    cwd release_path
  end
end
```

**Note**  
When you deploy an update to a JSP app, Tomcat might not recognize the update and instead continue to run the existing app version\. This can happen, for example, if you deploy your app as a \.zip file that contains only a JSP page\. To ensure that Tomcat runs the most recently deployed version, the project's root directory should include a WEB\-INF directory that contains a `web.xml` file\. A `web.xml` file can contain a variety of content, but the following is sufficient to ensure that Tomcat recognizes updates and runs the currently deployed app version\. You don't have to change the version for each update\. Tomcat will recognize the update even if the version hasn't changed\.  

```
<context-param>
  <param-name>appVersion</param-name>
  <param-value>0.1</param-value>
</context-param>
```

**Topics**
+ [Deploying a JSP App](#layers-java-deploy-jsp)
+ [Deploying a JSP App with a Back\-End Database](#layers-java-deploy-jsp-db)

## Deploying a JSP App<a name="layers-java-deploy-jsp"></a>

To deploy a JSP app, specify the name and repository information\. You can also optionally specify domains and SSL settings\. For more information on how to create an app, see [Adding Apps](workingapps-creating.md)\. The following procedure shows how to create and deploy a simple JSP page from a public Amazon S3 archive\. For information on how to use other repository types, including private Amazon S3 archives, see [Application Source](workingapps-creating.md#workingapps-creating-source)\.

The following example shows the JSP page, which simply displays some system information\.

```
<%@ page import="java.net.InetAddress" %>
<html>
<body>
<%
    java.util.Date date = new java.util.Date();
    InetAddress inetAddress = InetAddress.getLocalHost();
%>
The time is 
<%
    out.println( date );
    out.println("<br>Your server's hostname is "+inetAddress.getHostName());
%>
<br>
</body>
</html>
```

**Note**  
The following procedure assumes that you are already familiar with the basics of creating stacks, adding instances to layers, and so on\. If you are new to AWS OpsWorks Stacks, you should first see [Getting Started with Chef 11 Linux Stacks](gettingstarted.md)\.

**To deploy a JSP page from an Amazon S3 archive**

1. [Create a stack](workingstacks-creating.md) with a Java App Server layer, [add a 24/7 instance](workinginstances-add.md) to the layer, and [start it](workinginstances-starting.md)\. 

1. Copy the code to a file named `simplejsp.jsp`, put the file in a folder named `simplejsp`, and create a `.zip` archive of the folder\. The names are arbitrary; you can use any file or folder names that you want\. You can also use other types of archive, including gzip, bzip2, tarball, or Java WAR file\. Note that AWS OpsWorks Stacks does not support uncompressed tarballs\. To deploy multiple JSP pages, include them in the same archive\.

1. Upload the archive to an Amazon S3 bucket and make the file public\. Copy the file's URL for later use \. For more information on how to create buckets and upload files, go to [Get Started With Amazon Simple Storage Service](http://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html)\.

1. [Add an app](workingapps-creating.md#workingapps-creating-general) to the stack and specify following settings:
   + **Name** – `SimpleJSP`
   + **App type** – `Java`
   + **Repository type** – `Http Archive`
   + **Repository URL** – the Amazon S3 URL of your archive file\. It should look something like `https://s3.amazonaws.com/jsp_example/simplejsp.zip`\.

   Use the default values for the remaining settings and then click **Add App** to create the app\.

1. [Deploy the app](workingapps-deploying.md) to the Java App Server instance\.

You can now go to the app's URL and view the app\. If you have not specified a domain, you can construct a URL by using either the instance's public IP address or its public DNS name\. To get an instance's public IP address or public DNS name, go the AWS OpsWorks Stacks console and click the instance's name on the **Instances** page to open its details page\. 

The rest of URL depends on the app's short name, which is a lowercase name that AWS OpsWorks Stacks generates from the app name that you specified when you created the app\. For example the short name of SimpleJSP is simplejsp\. You can get an app's short name from its details page\. 
+ If the short name is `root`, you can use either `http://public_DNS/appname.jsp` or `http://public_IP/appname.jsp`\.
+ Otherwise, you can use either `http://public_DNS/app_shortname/appname.jsp` or `http://public_IP/app_shortname/appname.jsp`\. 

If you have specified a domain for the app, the URL is `http://domain/appname.jsp`\.

The URL for the example would be something like `http://192.0.2.0/simplejsp/simplejsp.jsp`\.

If you want to deploy multiple apps to the same instance, you should not use `root` as a short name\. This can cause URL conflicts that prevent the app from working properly\. Instead, assign a different domain name to each app\.

## Deploying a JSP App with a Back\-End Database<a name="layers-java-deploy-jsp-db"></a>

JSP pages can use a JDBC `DataSource` object to connect to a back end database\. You create and deploy such an app by using the procedure in the previous section, with one additional step to set up the connection\.

The following JSP page shows how to connect to a `DataSource` object\.

```
<html>
  <head>
    <title>DB Access</title>
  </head>
  <body>
    <%@ page language="java" import="java.sql.*,javax.naming.*,javax.sql.*" %>
    <%
      StringBuffer output = new StringBuffer();
      DataSource ds = null;
      Connection con = null;
      Statement stmt = null;
      ResultSet rs = null;
      try {
        Context initCtx = new InitialContext();
        ds = (DataSource) initCtx.lookup("java:comp/env/jdbc/mydb");
        con = ds.getConnection();
        output.append("Databases found:<br>");
        stmt = con.createStatement();
        rs = stmt.executeQuery("show databases");
        while (rs.next()) {
          output.append(rs.getString(1));
          output.append("<br>");
        }
      }
      catch (Exception e) {
        output.append("Exception: ");
        output.append(e.getMessage());
        output.append("<br>");
      }
      finally {
        try {
          if (rs != null) {
            rs.close();
          }
          if (stmt != null) {
            stmt.close();
          }
          if (con != null) {
            con.close();
          }
        }
        catch (Exception e) {
          output.append("Exception (during close of connection): ");
          output.append(e.getMessage());
          output.append("<br>");
        }
      }
    %>
    <%= output.toString() %>
  </body>
</html>
```

AWS OpsWorks Stacks creates and initializes the `DataSource` object, binds it to a logical name, and registers the name with a Java Naming and Directory Interface \(JNDI\) naming service\. The complete logical name is `java:comp/env/user-assigned-name`\. You must specify the user\-assigned part of the name by adding custom JSON attributes to the stack configuration and deployment attributes to define the `['opsworks_java']['datasources']` attribute, as described in the following\. 

**To deploy a JSP page that connects to a MySQL database**

1. [Create a stack](workingstacks-creating.md) with a Java App Server layer, [add 24/7 instances](workinginstances-add.md) to each layer, and [start it](workinginstances-starting.md)\. 

1. Add a database layer to the stack\. The details depend on which database you use\.

   To use a MySQL instance for the example, [add a MySQL layer](workinglayers-db-mysql.md) to the stack, [add a 24/7 instance](workinginstances-add.md) to the layer, and [start it](workinginstances-starting.md#workinginstances-starting-start)\.

   To use an Amazon RDS \(MySQL\) instance for the example:
   + Specify a MySQL database engine for the instance\.
   + Assign the **AWS\-OpsWorks\-DB\-Master\-Server \(*security\_group\_id*\)** and **AWS\-OpsWorks\-Java\-App\-Server \(*security\_group\_id*\)** security groups to the instance\. AWS OpsWorks Stacks creates these security groups for you when you create your first stack in the region\.
   + Create a database named `simplejspdb`\.
   + Ensure that the master user name and password do not contain `&` or other characters that could cause a Tomcat error\.

     In particular during startup Tomcat must parse the web app context file, which is an XML file that includes the master password and user name\. If the either string includes a `&` character, the XML parser treats it as a malformed XML entity and throws a parsing exception, which prevents Tomcat from starting\. For more information about the web app context file, see [tomcat::context](create-custom-configure.md#create-custom-configure-context)\.
   + [Add a MySQL driver](workingapps-connectdb.md) to the Java App Server layer\.
   + [Register the RDS instance](workinglayers-db-rds.md#workinglayers-db-rds-register) with your stack\. 

   For more information about how to use Amazon RDS instances with AWS OpsWorks Stacks, see [Amazon RDS Service Layer](workinglayers-db-rds.md)\.

1. Copy the example code to a file named `simplejspdb.jsp`, put the file in a folder named `simplejspdb`, and create a `.zip` archive of the folder\. The names are arbitrary; you can use any file or folder names that you want\. You can also use other types of archive, including gzip, bzip2, or tarball\. To deploy multiple JSP pages, include them in the same archive\. For information on how to deploy apps from other repository types, see [Application Source](workingapps-creating.md#workingapps-creating-source)\.

1. Upload the archive to an Amazon S3 bucket and make the file public\. Copy the file's URL for later use \. For more information on how to create buckets and upload files, go to [Get Started With Amazon Simple Storage Service](http://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html)\.

1. [Add an app](workingapps-creating.md#workingapps-creating-general) to the stack and specify following settings:
   + **Name** – `SimpleJSPDB`
   + **App type** – `Java`
   + **Data source type** – **OpsWorks** \(for a MySQL instance\) or **RDS** \(for an Amazon RDS instance\)\.
   + **Database instance** – The MySQL instance you created earlier, which is typically named **db\-master1\(mysql\)**, or the Amazon RDS instance, which will be named ***DB\_instance\_name* \(mysql\)**\.
   + **Database name** – `simplejspdb`\.
   + **Repository type** – `Http Archive`
   + **Repository URL** – the Amazon S3 URL of your archive file\. It should look something like `https://s3.amazonaws.com/jsp_example/simplejspdb.zip`\.

   Use the default values for the remaining settings and then click **Add App** to create the app\.

1. Add the following custom JSON attributes to the stack configuration attributes, where simplejspdb is the app's short name\.

   ```
   {
     "opsworks_java": {
       "datasources": {
         "simplejspdb": "jdbc/mydb" 
       }
     }
   }
   ```

   AWS OpsWorks Stacks uses this mapping to generate a context file with the necessary database information\.

   For more information on how to add custom JSON attributes to the stack configuration attributes, see [Using Custom JSON](workingstacks-json.md)\.

1. [Deploy the app](workingapps-deploying.md) to the Java App Server instance\.

You can now use the app's URL to view the app\. For a description of how to construct the URL, see [Deploying a JSP App](#layers-java-deploy-jsp)\. 

The URL for the example would be something like `http://192.0.2.0/simplejspdb/simplejspdb.jsp`\.

**Note**  
The `datasources` attribute can contain multiple attributes\. Each attribute is named with an apps short name and set to the appropriate user\-assigned part of a logical name\. If you have multiple apps, you can use separate logical names, which requires a custom JSON something like the following\.  

```
{
  "opsworks_java": {
    "datasources": {
      "myjavaapp": "jdbc/myappdb",
      "simplejsp": "jdbc/myjspdb",
      ...
    }
  }
}
```