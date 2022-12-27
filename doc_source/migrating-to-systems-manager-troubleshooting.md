# Troubleshooting<a name="migrating-to-systems-manager-troubleshooting"></a>

This section contains some common issues, and suggested solutions for those issues\.

**Topics**
+ [Provided principal is not valid](#w2ab1c14c39c19b7)
+ [Unable to delete CloudFormation stack when Auto Scaling group protected instances are enabled](#w2ab1c14c39c19b9)
+ [Access denied error when providing existing S3 bucket and prefix](#w2ab1c14c39c19c11)

## Provided principal is not valid<a name="w2ab1c14c39c19b7"></a>

**Problem:** You receive an error message stating that the principal you provided is not valid\.

**Cause:** This occurs because the Auto Scaling group doesn't have a service role\.

**Solution:** Create an Auto Scaling group in the Region where the error occurred\. Creating an Auto Scaling group creates the necessary service\-linked role for your custom termination policy\.

## Unable to delete CloudFormation stack when Auto Scaling group protected instances are enabled<a name="w2ab1c14c39c19b9"></a>

**Problem:** The `--enable-instance-protection` parameter is set to `TRUE` and some of your Auto Scaling group's EC2 instances are protected with the `protected_instance` tag key, which prevents your AWS CloudFormation stack from being completely deleted\.

**Cause:** The EC2 instances have a `protected_instance` tag key which protects them from termination events\. 

**Solution:** Remove the `protected_instance` tag key from the EC2 instances\. This allows the Auto Scaling group to scale down\. After the Auto Scaling group scales down, you can delete the AWS CloudFormation stack\. 

## Access denied error when providing existing S3 bucket and prefix<a name="w2ab1c14c39c19c11"></a>

**Problem:** You receive an `AccessDenied` error when you provide an existing S3 bucket and prefix\.

**Cause:** The S3 bucket policy does not provide the necessary permissions to deliver the load balancer logs to the bucket\.

**Solution:** Update the S3 bucket policy to allow the script to deliver the load balancer access logs to the bucket\. For more information about how to update the bucket policy, see [Enable access logs for your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/enable-access-logging.html) in the *Elastic Load Balancing: Application Load Balancers User Guide*\.