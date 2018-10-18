# Create an Amazon RDS MySQL Database<a name="customizing-rds-connect-create"></a>

Now you're ready to create an RDS database for the example using the Amazon RDS console's Launch DB Instance Wizard\. The following procedure is a brief summary of the essential details\. For a detailed description of how to create a database, see [Getting Started with Amazon RDS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html)\.

**To create the Amazon RDS database**

1. If this is your first time creating an RDS database, click **Get Started Now**\. Otherwise, click **RDS Dashboard** in the navigation pane, and then click **Launch a DB Instance**\.

1. Select the **MySQL Community Edition** as the DB instance\.

1. For **Do you plan to use this database for production purposes?** select **No, this instance\.\.\.**, which is sufficient for the example\. For production use, you might want to select **Yes, use Multi\-AZ Deployment\.\.\.**\. Click **Next Step**\.

1. On the **Specify DB Details** page, specify the following settings:
   + **DB Instance Class**: **db\.t2\.micro**
   + **Multi\-AZ Deployment**: **No**
   + **Allocated Storage**: **5** GB
   + **DB Instance Identifier**: **rdsexample**
   + **Master Username**: **opsworksuser**
   + **Master Password**: Specify a suitable password and record it for later use\.

   Accept the default settings for the other options and click **Next Step**\.

1. On the **Configure Advanced Settings** page, specify the following settings:
   + In the **Network & Security** section, for **VPC Security Group\(s\)**, select **phpsecgroup \(VPC\)**
   + In the **Database Options** section, for **Database Name**, type **rdsexampledb**
   + In the **Backup** section, set **Backup Retention Period** to **0** for the purposes of this walkthrough\.

   Accept the default settings for the other options and click **Launch DB Instance**\.

1. Choose **View Your DB Instances** to see the list of DB instances\.

1. Select the **rdsexample** instance in the list and click the arrow to reveal the instance endpoint and other details\. Record the endpoint for later use\. It will be something like `rdsexample.c6c8mntzhgv0.us-west-2.rds.amazonaws.com:3306`\. Just record the DNS name; you won't need the port number\.

1. Use a tool such as MySQL Workbench to create a table named `urler` in the `rdsexampledb` database by using following SQL command:

   ```
   CREATE TABLE urler(id INT UNSIGNED NOT NULL AUTO_INCREMENT,author VARCHAR(63) NOT NULL,message TEXT,PRIMARY KEY (id))
   ```