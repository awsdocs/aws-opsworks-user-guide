# Example register Commands<a name="registered-instances-register-registering-examples"></a>

**Note**  
This feature is supported only for Linux stacks\.

This section contains some examples of `register` command strings\.

**Register an Amazon EC2 Instance from a Workstation**  <a name="registered-instances-register-registering-examples-workstation-ec2"></a>
The following example registers an Amazon EC2 instance from a workstation\. The command string uses default credentials and identifies the instance by its Amazon EC2 instance ID\. You can use the example for on\-premises instances by changing `ec2` to `on-premises`\.  

```
aws opsworks register \
  --region us-west-2 \
  --use-instance-profile \
  --infrastructure-class ec2 \
  --stack-id ad21bce6-7623-47f1-bf9d-af2affad8907 \
  --ssh-user-name my-sshusername \
  --ssh-private-key "./keys/mykeys.pem" \
  i-2422b9c5
```

**Register an On\-Premises Instance from a Workstation**  <a name="registered-instances-register-registering-examples-workstation-onprem"></a>
The following example registers an on\-premises instance from a separate workstation\. The command string uses default credentials and logs in to the instance with the specified `ssh` command string\. If your instance requires a password, `register` prompts you\. You can use the example for Amazon EC2 instances by changing `on-premises` to `ec2`\.   

```
aws opsworks register \
  --region us-west-2 \
  --infrastructure-class on-premises \
  --stack-id ad21bce6-7623-47f1-bf9d-af2affad8907 \
  --override-ssh "ssh your-user@192.0.2.0"
```
You can use `--override-ssh` to specify any custom SSH command string\. AWS OpsWorks Stacks then uses the specified string to log in to the instance instead of constructing a command string\. For another example, see [Register an Instance Using a Custom SSH Command String](#registered-instances-register-registering-examples-custom-ssh)\.

**Register an Instance Using a Custom SSH Command String**  <a name="registered-instances-register-registering-examples-custom-ssh"></a>
The following example registers an on\-premises instance from a workstation, and uses the `--override-ssh` argument to specify a custom SSH command that `register` uses to log in to the instance\. This example uses `sshpass` to log in with a user name and password, but you can specify any valid `ssh` command string\.  

```
aws opsworks register \
  --region us-west-2 \
  --infrastructure-class on-premises \
  --stack-id 2f92ff9d-04f2-4728-879b-f4283b40783c \
  --override-ssh "sshpass -p 'mypassword' ssh your-user@192.0.2.0"
```

**Register an Instance by Running `register` from the Instance**  <a name="registered-instances-register-registering-examples-local"></a>
The following example shows how to register an Amazon EC2 instance by running `register` from the instance itself\. The command string depends on default credentials for its permissions\. To use the example for an on\-premises instance, change `--infrastructure-class` to `on-premises`\.  

```
aws opsworks register \
  --region us-west-2 \
  --infrastructure-class ec2 \
  --stack-id ad21bce6-7623-47f1-bf9d-af2affad8907 \
  --local
```

**Register an Instance with a Private IP Address**  <a name="registered-instances-register-registering-examples-private-ip"></a>
By default, `register` uses the instance's public IP address to log in to the instance\. To register an instance with a private IP address, such as an instance in a VPC's private subnet, you must use `--override-ssh` to specify a custom `ssh` command string\.  

```
aws opsworks register \
  --region us-west-2 \
  --infrastructure-class ec2 \
  --stack-id 2f92ff9d-04f2-4728-879b-f4283b40783c \
  --override-ssh "ssh -i mykey.pem ec2-user@10.183.201.93" \
  i-2422b9c5
```