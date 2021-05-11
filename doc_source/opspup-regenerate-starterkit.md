# Regenerate the starter kit for an AWS OpsWorks for Puppet Enterprise server<a name="opspup-regenerate-starterkit"></a>

The starter kit for OpsWorks for Puppet Enterprise contains a README file with examples and a private key for the administrator\. You can regenerate the starter kit for an OpsWorks for Puppet Enterprise master server by running commands in the AWS CLI\. You cannot regenerate the starter kit for an OpsWorks for Puppet Enterprise server by using the console\.

For more information about how to use the starter kit, see [Configure the Puppet Master Using the Starter Kit](opspup-starterkit.md)\.

## Regenerate the OpsWorks for Puppet Enterprise starter kit with the AWS CLI<a name="opspup-regenerate-starterkit-cli"></a>

**Note**  
When you regenerate the starter kit, you also regenerate the private key for the OpsWorks for Puppet Enterprise server\. Optionally, you can change the value of `PUPPET_ADMIN_PASSWORD` when you regenerate the starter kit\. If you change the password, it must be between 8 and 32 ASCII characters\. Because you must specify at least one attribute value to run update\-server\-engine\-attributes, if you do not want to change the value of `PUPPET_ADMIN_PASSWORD`, specify an attribute value of `null`\.

Regenerate the starter kit by running the [https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/update-server-engine-attributes.html](https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/update-server-engine-attributes.html) command\. In an AWS CLI session, run the following command\. Specify your OpsWorks for Puppet Enterprise server name as the value of `--server-name`\. To set a password of your own as the value of `PUPPET_ADMIN_PASSWORD`, specify the value of the new password in `--attribute-value`\. Otherwise, set `--attribute-value` to null\.

```
aws opsworks-cm update-server-engine-attributes \
   --server-name server_name \
   --attribute-name "PUPPET_ADMIN_PASSWORD" \
   --attribute-value new_password
```

The following command is an example that specifies a new password that the server's administrator wants to use\.

```
aws opsworks-cm update-server-engine-attributes \
   --server-name test-puppet-server \
   --attribute-name "PUPPET_ADMIN_PASSWORD" \
   --attribute-value "zZZzDj2DLYXSZFRv1d"
```

The output of this command is information about the server, and a base64\-encoded ZIP file\. The ZIP file contains a starter kit, which includes a README and the required RSA private key\. Save this file, unzip it, and then change to the directory where you've unzipped the file contents\.