+++
title = "Clean up resources"
date = 2020
weight = 4
chapter = false
pre = "<b>4. </b>"
+++
You clean up resources in the following order:

#### Delete the sample web
1. Go to [CloudFormation Console](https://console.aws.amazon.com/cloudformation/home).
* Select **WAFWorkshopSampleWebApp**.
* Click **Delete**.
![Delete the sample web app](/images/4-cleanupresource/cleanupresource-001.png?featherlight=false&width=90pc)
2. Click **Delete stack** to delete.
![Delete the sample web app](/images/4-cleanupresource/cleanupresource-002.png?featherlight=false&width=90pc)
#### Delete Web ACL
1. Go to [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start?region=global).
* Click **Web ACLs**.
* Select **waf-workshop-juice-shop**.
* Click **Delete**.
![Delete Web ACL](/images/4-cleanupresource/cleanupresource-003.png?featherlight=false&width=90pc)
2. Type ```delete``` to confirm, then click **Delete** to delete.
![Delete Web ACL](/images/4-cleanupresource/cleanupresource-004.png?featherlight=false&width=90pc)
#### Delete Kinesis
1. Go to [Amazon Kinesis Console](https://us-east-1.console.aws.amazon.com/kinesis/home?region=us-east-1#/home).
* Click **Delivery streams**.
* Select **aws-waf-logs-workshop-26**.
* Click **Delete**.
![Delete Kinesis](/images/4-cleanupresource/cleanupresource-005.png?featherlight=false&width=90pc)
2. Type ```aws-waf-logs-workshop-26``` to confirm, then click **Delete** to delete.
![Delete Kinesis](/images/4-cleanupresource/cleanupresource-006.png?featherlight=false&width=90pc)
#### Delete S3 bucket
1. Go to [**giao diện quản trị dịch vụ S3**](https://s3.console.aws.amazon.com/s3/).
* You would clean up resources in the following order: **aws-waf-logs-001**.
* Click Empty.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-007.png?featherlight=false&width=90pc)
2. Type ```permanently delete``` to confirm, then click **Empty** to delete all the data in this S3 bucket.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-008.png?featherlight=false&width=90pc)
* Click **Exit** to back to the S3 interface.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-009.png?featherlight=false&width=90pc)
3. Select S3 bucket **aws-waf-logs-001**, then click **Delete**.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-010.png?featherlight=false&width=90pc)
4. Type the name of the bucket, then click **Delete bucket** to delete the S3 bucket.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-011.png?featherlight=false&width=90pc)