# Using Git Repository SSH Keys<a name="workingapps-deploykeys"></a>

A Git repository SSH key, sometimes called a deploy SSH key, is an SSH key with no password that provides access to a private Git repository\. Ideally, it doesn't belong to any specific developer\. Its purpose is to allow AWS OpsWorks Stacks to asynchronously deploy apps or cookbooks from a Git repository without requiring any further input from you\.

The following describes the basic procedure for creating a repository SSH key\. For details, see the documentation for your repository\. For example, [Managing deploy keys](https://help.github.com/articles/managing-deploy-keys) describes how to create a repository SSH key for a GitHub repository, and [Deployment Keys on Bitbucket](http://blog.bitbucket.org/2012/06/20/deployment-keys/) describes how to create a repository SSH key for a Bitbucket repository\. Note that some documentation describes creating a key on a server\. For AWS OpsWorks Stacks, just replace "server" with "workstation" in the instructions\. 

**To create a repository SSH key**

1. Create a deploy SSH key pair for your Git repository on your workstation using a program such as `ssh-keygen`\. 
**Important**  
AWS OpsWorks Stacks does not support SSH key passphrases\.

1. Assign the public key to the repository and store the private key on your workstation\.

1. Enter the private key in the **Repository SSH Key** box when you add an app or specify cookbook repository\. For more information, see [Adding Apps](workingapps-creating.md)\.

AWS OpsWorks Stacks passes the repository SSH key to each instance, and the built\-in recipes then use the key to connect to the repository and download the code\. The key is stored in the [`deploy` attributes](workingcookbook-json.md) as [`node[:deploy]['appshortname'][:scm][:ssh_key]`](attributes-json-deploy.md#attributes-json-deploy-app-scm-key), and is accessible only to the root user\. 