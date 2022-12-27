# Step 8: Review the instance<a name="migrating-to-systems-manager-review-instance"></a>

After you've started an instance, verify it runs as expected\.

1.  Review the Chef `startup` and `terminate` logs located in the S3 bucket specified by the script's `--command-logs-bucket` parameter\. By default, the logs are stored in a bucket with the name `aws-opsworks-application-manager-logs-account-id`\.

   1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1.  Choose the bucket containing your logs\.

   1.  Navigate to the `ApplyChefRecipes` prefix to view your logs\. 

1. Check Application Load Balancer connectivity and health\.

   Take the following steps to view the access logs for your load balancer\. You can specify the S3 bucket where you want to store the load balancer access logs by using the script's `--lb-access-logs-path` parameter\.

   1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1.  Choose your S3 bucket, and then navigate to the prefix containing your logs\. 

1.  Verify the instance passes all Auto Scaling and Application Load Balancer health checks \(if you've configured any\)\. 

   You can view information about Auto Scaling health on the new **Instances** tab\.

   1.  Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. 

   1. In the navigation pane, choose **Application Manager**\.

   1.  In the **Applications** section, choose **Custom applications**\. 

   1.  Choose the application in the list\. Application Manager opens the **Overview** tab\. 

   1.  Choose the **Instances** tab to view information about Auto Scaling health\.

After you verify that Chef recipes run successfully, you can decrease the Auto Scaling group capacity to terminate the instance\. If you have any custom termination recipes, verify the recipes operate as expected\.