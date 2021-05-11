# Regenerate the starter kit for an AWS OpsWorks for Chef Automate server<a name="opscm-regenerate-starterkit"></a>

The starter kit for AWS OpsWorks for Chef Automate contains a README file with examples, a `knife.rb` configuration file, and a private key for the primary, or pivotal, user\. A new key pair is generated—and the old key is reset—each time you download the starter kit\. You can regenerate the starter kit for an AWS OpsWorks for Chef Automate server in one of two ways:
+ In the AWS OpsWorks console, on the **Actions** menu of the details page for an AWS OpsWorks for Chef Automate server\. You are prompted to confirm whether you want to regenerate and reset the old pivotal key\.
+ By running commands in the AWS CLI\.

For more information about how to use the starter kit, see [Configure the Chef Server Using the Starter Kit](opscm-starterkit.md)\.

## Regenerate the AWS OpsWorks for Chef Automate starter kit with the AWS CLI<a name="opscm-regenerate-starterkit-cli"></a>

**Note**  
When you regenerate the starter kit, you also regenerate and reset the authentication key pair for your Chef Automate server, and delete the current key pair\.

Regenerate the starter kit by running the [https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/update-server-engine-attributes.html](https://docs.aws.amazon.com/cli/latest/reference/opsworks-cm/update-server-engine-attributes.html) command\. In an AWS CLI session, run the following command\. Specify your server name as the value of `--server-name`\. To set a public key of your own as the value of `CHEF_AUTOMATE_PIVOTAL_KEY`, specify the value of the public key in `--attribute-value`\. Otherwise, set `--attribute-value` to null\.

```
aws opsworks-cm update-server-engine-attributes \
   --server-name server_name \
   --attribute-name "CHEF_AUTOMATE_PIVOTAL_KEY" \
   --attribute-value your_public_key
```

The following command is an example that specifies a public key value that the server's administrator wants to use\.

```
aws opsworks-cm update-server-engine-attributes \
   --server-name your-test-server \
   --attribute-name "CHEF_AUTOMATE_PIVOTAL_KEY" \
   --attribute-value "-----BEGIN PUBLIC KEY-----ExamplePublicKey-----END PUBLIC KEY-----"
```

The following command is an example that lets AWS OpsWorks for Chef Automate regenerate the public key\.

```
aws opsworks-cm update-server-engine-attributes \
   --server-name your-test-server \
   --attribute-name "CHEF_AUTOMATE_PIVOTAL_KEY" \
   --attribute-value null
```

The output of this command is information about the server, and a base64\-encoded ZIP file\. The ZIP file contains a Chef starter kit, which includes a README, a configuration file, and the required RSA private key\. Save this file, unzip it, and then change to the directory where you've unzipped the file contents\. From this directory, you can run `knife` commands\.