# Getting Started with OpsWorks for Puppet Enterprise<a name="gettingstarted-opspup"></a>

OpsWorks for Puppet Enterprise lets you run a [Puppet Enterprise](https://puppet.com/products/puppet-enterprise) server in AWS\. You can provision a Puppet Enterprise master server in about 15 minutes\.

The following walkthrough helps you create your first Puppet master in OpsWorks for Puppet Enterprise\.

## Prerequisites<a name="gettingstarted-opspup-prereqs"></a>

**Topics**
+ [Get an AWS Account and Your Root User Credentials](#getting-started-signup)
+ [Install the Puppet Development Kit](#w4ab1b7c19b7c11)
+ [Install the Puppet Enterprise Client Tools](#w4ab1b7c19b7c13)
+ [Set Up a Git Control Repository](#configure-control-repository)
+ [Set Up a VPC](#set-up-vpc-puppet)
+ [Set Up an EC2 Key Pair \(Optional\)](#w4ab1b7c19b7c19)

First, create the resources outside of OpsWorks for Puppet Enterprise that you'll need to access and manage your Puppet master\. If you already have an AWS account set up, skip to [Set Up a VPC](#set-up-vpc-puppet)\.

### Get an AWS Account and Your Root User Credentials<a name="getting-started-signup"></a>

To access AWS, you must sign up for an AWS account\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

 AWS sends you a confirmation email after the sign\-up process is complete\. At any time, you can view your current account activity and manage your account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **My Account**\.

Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them from the AWS Management Console\. As a best practice, do not use the AWS account root user access keys for any task where it's not required\. Instead, [create a new administrator IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) with access keys for yourself\.

The only time that you can view or download the secret access key is when you create the keys\. You cannot recover them later\. However, you can create new access keys at any time\. You must also have permissions to perform the required IAM actions\. For more information, see [Permissions Required to Access IAM Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions-required.html) in the *IAM User Guide*\.

**To create access keys for an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. Choose the name of the user whose access keys you want to create, and then choose the **Security credentials** tab\.

1. In the **Access keys** section, choose **Create access key**\.

1. To view the new access key pair, choose **Show**\. You will not have access to the secret access key again after this dialog box closes\. Your credentials will look something like this:
   + Access key ID: AKIAIOSFODNN7EXAMPLE
   + Secret access key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

1. To download the key pair, choose **Download \.csv file**\. Store the keys in a secure location\. You will not have access to the secret access key again after this dialog box closes\.

   Keep the keys confidential in order to protect your AWS account and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. No one who legitimately represents Amazon will ever ask you for your secret key\.

1. After you download the `.csv` file, choose **Close**\. When you create an access key, the key pair is active by default, and you can use the pair right away\.

**Related topics**
+ [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*
+ [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in *AWS General Reference* 

### Install the Puppet Development Kit<a name="w4ab1b7c19b7c11"></a>

1. From the Puppet website, [download the Puppet Development Kit](https://puppet.com/download-puppet-development-kit) that matches your local computer's operating system\.

1. Install the Puppet Development Kit\.

1. Add the Puppet Development Kit to your local computer's `PATH` variable\.
   + On a Linux or macOS operating system, you can add the Puppet Development Kit to your `PATH` variable by running the following command in a Bash shell\.

     ```
     echo 'export PATH=/opt/puppetlabs/pdk/bin/pdk:$PATH' >> ~/.bash_profile && source ~/.bash_profile
     ```
   + On a Windows\-based operating system, you can add the Puppet Development Kit to your `PATH` variable by using the following \.NET Framework command in a PowerShell session, or in the **Environment Variables** dialog box accessible from **System Properties**\. You may need to run your PowerShell session as an administrator to run the following command\.

     ```
     [Environment]::SetEnvironmentVariable("Path","new path value","Machine")
     ```

### Install the Puppet Enterprise Client Tools<a name="w4ab1b7c19b7c13"></a>

Puppet Enterprise \(PE\) client tools are a set of command\-line tools that let you access Puppet Enterprise services from your workstation\. The tools can be installed on many different operating systems, and they can also be installed on nodes that you are managing by using Puppet\. For information about supported operating systems for the tools, and how to install them, see [Installing PE client tools](https://puppet.com/docs/pe/2017.3/installing/installing_pe_client_tools.html) in the Puppet Enterprise documentation\.

### Set Up a Git Control Repository<a name="configure-control-repository"></a>

Before you can launch a Puppet master, you must have a control repository configured in Git to store and change\-manage your Puppet modules and classes\. A URL to a Git repository and HTTPS or SSH account information to access the repository are required in the steps to launch your Puppet Enterprise master server\. For more information about how to set up a control repository that your Puppet Enterprise master will use, see [Setting up a control repository](https://puppet.com/docs/pe/2017.3/code_management/control_repo.html)\. You can also find control repository setup instructions in the readme for Puppet's [`control-repo` sample repository on GitHub](https://github.com/puppetlabs/control-repo)\. The structure of the control repository resembles the following\.

```
├── LICENSE
├── Puppetfile
├── README.md
├── environment.conf
├── hieradata
│   ├── common.yaml
│   └── nodes
│       └── example-node.yaml
├── manifests
│   └── site.pp
├── scripts
│   ├── code_manager_config_version.rb
│   ├── config_version.rb
│   └── config_version.sh
└── site
    ├── profile
    │   └── manifests
    │       ├── base.pp
    │       └── example.pp
    └── role
        └── manifests
            ├── database_server.pp
            ├── example.pp
            └── webserver.pp
```

#### Setting up a repository by using CodeCommit<a name="w4ab1b7c19b7c15b7"></a>

You can create a new repository by using CodeCommit\. For more information about how to use CodeCommit to create your control repository, see [Optional: Use CodeCommit as a Puppet r10k Remote Control Repository](opspup-puppet-codecommit.md) in this guide\. For more information about how to get started with Git on CodeCommit, see [Getting started with AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/getting-started.html)\. To authorize your OpsWorks for Puppet Enterprise server for your repository, attach the `AWSCodeCommitReadOnly` policy to your IAM instance profile role\.

### Set Up a VPC<a name="set-up-vpc-puppet"></a>

Your OpsWorks for Puppet Enterprise master must operate in an Amazon Virtual Private Cloud\. You can add it to an existing VPC, use the default VPC, or create a new VPC to contain the server\. For information about Amazon VPC and how to create a new VPC, see the [Amazon VPC Getting Started Guide](https://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/)\.

If you create your own VPC, or use an existing one, the VPC should have the following settings or properties\.
+ The VPC should have at least one subnet\.

  If your OpsWorks for Puppet Enterprise master will be publicly accessible, make the subnet public, and enable **Auto\-assign public IP**\.
+ **DNS resolution** should be enabled\.
+ On the subnet, enable **Auto\-assign public IP**\.

If you are unfamiliar with creating VPCs or running your instances in them, you can run the following AWS CLI command to create a VPC with a single public subnet, by using an AWS CloudFormation template that AWS OpsWorks provides for you\. If you prefer to use the AWS Management Console, you can also upload the [template](https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-vpc.yaml) to the AWS CloudFormation console\.

```
aws cloudformation create-stack --stack-name OpsWorksVPC --template-url https://s3.amazonaws.com/opsworks-cm-us-east-1-prod-default-assets/misc/opsworks-cm-vpc.yaml
```

### Set Up an EC2 Key Pair \(Optional\)<a name="w4ab1b7c19b7c19"></a>

An SSH connection is not necessary or recommended for typical management of the Puppet server; you can use the AWS Management Console and AWS CLI commands to perform many management tasks on your Puppet server\.

An EC2 key pair is required to connect to your server by using SSH in the event that you lose or want to change the sign\-in password for the Puppet Enterprise web\-based console\. You can use an existing key pair, or create a new key pair\. For more information about how to create a new EC2 key pair, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

If you don't need an EC2 key pair, you are ready to create a Puppet Enterprise master\.