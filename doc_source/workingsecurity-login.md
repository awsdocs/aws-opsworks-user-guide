# Signing in as an IAM User<a name="workingsecurity-login"></a>

Each account has a URL called an account alias, which IAM users use to sign in to the account\. The URL is on your IAM console's **Dashboard \(Getting Started\)** page under **Account Alias**\.

When creating a new IAM user, an administrative user specifies or has the IAM service generate the following:
+ A user name
+ An optional password

  You can specify a password or have IAM generate one for you\.
+ An optional Access Key ID and Secret Access Key

  IAM generates a new key pair for each user\. The keys are required for users who need to access AWS OpsWorks Stacks using the API or CLI\. They are not required for console\-only users\.

The administrative user then provides this information and the account alias to each new user, for example, in an e\-mail\.

**To sign in to AWS OpsWorks Stacks as an IAM user**

1. In your browser, navigate to the account alias URL\.

1. Enter your account name, user name, and password and click **Sign in**\.

1. In the navigation bar, click **Services** and select **OpsWorks**\.