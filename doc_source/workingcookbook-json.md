# Stack Configuration and Deployment Attributes<a name="workingcookbook-json"></a>

When AWS OpsWorks Stacks runs a command on an instance—for example, a deploy command in response to a Deploy lifecycle event—it adds a set of attributes to the instance's node object that describes the stack's current configuration\. For Deploy events and [Execute Recipes stack commands](workingstacks-commands.md), AWS OpsWorks Stacks installs deploy attributes, which provide some additional deployment information\. For more information about the node object, see [Overriding Attributes](workingcookbook-attributes.md)\. For a list of commonly used stack configuration and deployment attributes, including fully qualified node names, see [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md) and [Built\-in Cookbook Attributes](attributes-recipes.md)\.

**Note**  
On Linux stacks, you can get a complete list of these attributes, formatted as a JSON object, by using the agent CLI's [get\_json command](agent-json.md)\. 

The following sections show the attributes associated with a Configure event and a Deploy event for a simple stack, which consists of the following:
+ A PHP App Server layer with two instances
+ An HAProxy layer with one instance

The examples are from one of the PHP App Server instances, **php\-app1**\. For convenience, the attributes are formatted as a JSON object\. The object's structure maps to the attributes' fully qualified names\. For example, the `node[:opsworks][:ruby_version]` attribute appears as follows in a JSON representation\.

```
{
  "opsworks": {
    ...
    "ruby_version": "1.8.7",
    ...
  }
}
```

**Topics**
+ [Configure Attributes](#workingcookbook-json-configure)
+ [Deployment Attributes](#workingcookbook-json-deploy)

## Configure Attributes<a name="workingcookbook-json-configure"></a>

The following JSON object shows the attributes for a Configure event, which occurs on every instance in the stack when an instance comes online or goes offline\. The attributes include the built\-in stack configuration attributes and any [custom JSON attributes](workingstacks-json.md) that were defined for the stack prior to the event \(none in this example\)\. It has been edited for length\. For a detailed description of the various attributes, see [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md) and [Built\-in Cookbook Attributes](attributes-recipes.md)\.

```
{
  "opsworks": {
    "layers": {
      "php-app": {
        "id": "4a2a56c8-f909-4b39-81f8-556536d20648",
        "instances": {
          "php-app2": {
            "elastic_ip": null,
            "region": "us-west-2",
            "booted_at": "2013-02-26T20:41:10+00:00",
            "ip": "192.0.2.0",
            "aws_instance_id": "i-34037f06",
            "availability_zone": "us-west-2a",
            "instance_type": "c1.medium",
            "private_dns_name": "ip-10-252-0-203.us-west-2.compute.internal",
            "private_ip": "10.252.0.203",
            "created_at": "2013-02-26T20:39:39+00:00",
            "status": "online",
            "backends": 8,
            "public_dns_name": "ec2-192-0-2-0.us-west-2.compute.amazonaws.com"
          },
          "php-app1": {
            ...
          }
        },
        "name": "PHP Application Server"
      },
      "lb": {
        "id": "15c86142-d836-4191-860f-f4d310440f14",
        "instances": {
          "lb1": {
           ...
          }
        },
        "name": "Load Balancer"
      }
    },
    "agent_version": "104",
    "applications": [

    ],
    "stack": {
      "name": "MyStack"
    },
    "ruby_version": "1.8.7",
    "sent_at": 1361911623,
    "ruby_stack": "ruby_enterprise",
    "instance": {
      "layers": [
        "php-app"
      ],
      "region": "us-west-2",
      "ip": "192.0.2.0",
      "id": "45ef378d-b87c-42be-a1b9-b67c48edafd4",
      "aws_instance_id": "i-32037f00",
      "availability_zone": "us-west-2a",
      "private_dns_name": "ip-10-252-84-253.us-west-2.compute.internal",
      "instance_type": "c1.medium",
      "hostname": "php-app1",
      "private_ip": "10.252.84.253",
      "backends": 8,
      "architecture": "i386",
      "public_dns_name": "ec2-192-0-2-0.us-west-2.compute.amazonaws.com"
    },
    "activity": "configure",
    "rails_stack": {
      "name": null
    },
    "deployment": null,
    "valid_client_activities": [
      "reboot",
      "stop",
      "setup",
      "configure",
      "update_dependencies",
      "install_dependencies",
      "update_custom_cookbooks",
      "execute_recipes"
    ]
  },
  "opsworks_custom_cookbooks": {
    "recipes": [

    ],
    "enabled": false
  },
  "recipes": [
    "opsworks_custom_cookbooks::load",
    "opsworks_ganglia::configure-client",
    "ssh_users",
    "agent_version",
    "mod_php5_apache2::php",
    "php::configure",
    "opsworks_stack_state_sync",
    "opsworks_custom_cookbooks::execute",
    "test_suite",
    "opsworks_cleanup"
  ],
  "opsworks_rubygems": {
    "version": "1.8.24"
  },
  "ssh_users": {
  },
  "opsworks_bundler": {
    "manage_package": null,
    "version": "1.0.10"
  },
  "deploy": {
  }
}
```

Most of the information is under the `opsworks` attribute, which is often referred to as a namespace\. The following list describes the key attributes:
+ `layers` attributes – A set of attributes, each of which describes the configuration of one of the stack's layers\.

  The layers are identified by their shortnames, `php-app` and `lb` for this example\. For more information about shortnames for other layers, see [AWS OpsWorks Stacks Layer Reference](layers.md)\. 
+ `instances` attributes – Every layer has an `instances` element, which includes an attribute for each of the layers' online instances, named with the instance's short name\.

  The PHP App Server layer has two instances, `php-app1` and `php-app2`\. The HAProxy layer has one instance, `lb1`\. 
**Note**  
The `instances` element contains only those instances that are in the online state when the particular stack and deployment attributes are created\.
+ Instance attributes – Each instance attribute contains a set of attributes that characterize the instance, such as the instance's private IP address and private DNS name\. For brevity, the example shows only the `php-app2` attribute in detail; the others contain similar information\.
+ `applications` – A list of deployed apps, not used in this example\.
+ `stack` – The stack name; `MyStack` in this example\.
+ `instance` – The instance that these attributes are installed on; `php-app1` in this example\. Recipes can use this attribute to obtain information about the instance that they are running on, such as the instance's public IP address\.
+ `activity` – The activity that produced the attributes; a Configure event in this example\.
+ `rails_stack` – The Rails stack for stacks that include a Rails App Server layer\.
+ `deployment` – Whether these attributes are associated with a deployment\. It is set to `null` for this example because they are associated with a Configure event\.
+ `valid_client_activities` – A list of valid client activities\.

The `opsworks` attribute is followed by several other top\-level attributes, including the following:
+ `opsworks_custom_cookbooks` – Whether custom cookbooks are enabled\. If so, the attribute includes a list of custom recipes\.
+ `recipes` – The recipes that were run by this activity\.
+ `opsworks_rubygems` – The instance's RubyGems version\.
+ `ssh_users` – A list of SSH users; none in this example\.
+ `opsworks_bundler` – The bundler version and whether it is enabled\.
+ `deploy` – Information about deployment activities; none in this example\.

## Deployment Attributes<a name="workingcookbook-json-deploy"></a>

The attributes for a Deploy event or [Execute Recipes stack command](workingstacks-commands.md) consist of the built\-in stack configuration and deployment attributes, and any custom stack or deployment attributes \(none for this example\)\. The following JSON object shows the attributes from **php\-app1** that are associated with a Deploy event that deployed the SimplePHP app to the stack's PHP instances\. Much of the object consists of stack configuration attributes that are similar to the ones for the Configure event described in the previous section, so the example focuses primarily on the deployment\-specific attributes\. For a detailed description of the various attributes, see [Stack Configuration and Deployment Attributes: Linux](attributes-json-linux.md) and [Built\-in Cookbook Attributes](attributes-recipes.md)\.

```
{
   ...
  "opsworks": {
    ...
    "activity": "deploy",
    "applications": [
      {
        "slug_name": "simplephp",
        "name": "SimplePHP",
        "application_type": "php"
      }
    ],
    "deployment": "5e6242d7-8111-40ee-bddb-00de064ab18f",
    ...
  },
  ...
{
  "ssh_users": {
  },
  "deploy": {
    "simplephpapp": {
      "application": "simplephpapp",
      "application_type": "php",
      "environment_variables": {
        "USER_ID": "168424",
        "USER_KEY": "somepassword"
      },
      "auto_bundle_on_deploy": true,
      "deploy_to": "/srv/www/simplephpapp",
      "deploying_user": "arn:aws:iam::123456789012:user/guysm",
      "document_root": null,
      "domains": [
        "simplephpapp"
      ],
      "migrate": false,
      "mounted_at": null,
      "rails_env": null,
      "restart_command": "echo 'restarting app'",
      "sleep_before_restart": 0,
      "ssl_support": false,
      "ssl_certificate": null,
      "ssl_certificate_key": null,
      "ssl_certificate_ca": null,
      "scm": {
        "scm_type": "git",
        "repository": "git://github.com/amazonwebservices/opsworks-demo-php-simple-app.git",
        "revision": "version1",
        "ssh_key": null,
        "user": null,
        "password": null
      },
      "symlink_before_migrate": {
        "config/opsworks.php": "opsworks.php"
      },
      "symlinks": {
      },
      "database": {
      },
      "memcached": {
        "host": null,
        "port": 11211
      },
      "stack": {
        "needs_reload": false
      }
    }
  },
}
```

The `opsworks` attribute is largely identical to the example in the previous section\. The following sections are most relevant to deployment:
+ `activity` – The event that is associated with these attributes; a Deploy event in this example\.
+ `applications` – Contains a set of attributes for each app that provide the apps' names, slug names, and types\.

  The slug name is the app's short name, which AWS OpsWorks Stacks generates from the app name\. The slug name for SimplePHP is simplephp\.
+ `deployment` – The deployment ID, which uniquely identifies a deployment\.

The `deploy` attribute includes information about the apps that are being deployed\. For example, the built\-in Deploy recipes use the data in the `deploy` attribute to install files in the appropriate directories and create database connection files\. The `deploy` attribute includes one attribute for each deployed app, named with the app's short name\. Each app attribute includes the following attributes:
+ `environment_variables` – Contains any environment variables that you have defined for the app\. For more information, see [Environment Variables](workingapps-creating.md#workingapps-creating-environment)\.
+ `domains` – By default, the domain is the app's short name, which is simplephpapp for this example\. If you have assigned custom domains, they appear here as well\. For more information, see [Using Custom Domains](workingapps-domains.md)\.
+ `application` – The app's short name\.
+ `scm` – This element contains the information required to download the app's files from its repository; a Git repository in this example\.
+ `database` – Database information, if the stack includes a database layer\.
+ `document_root` – The document root, which is set to `null` in this example, indicating that the root is public\.
+ `ssl_certificate_ca`, `ssl_support`, `ssl_certificate_key` – Indicates whether the app has SSL support\. If so, the `ssl_certificate_key` and `ssl_certificate_ca` attributes are set to the corresponding certificates\.
+ `deploy_to` – The app's root directory\.