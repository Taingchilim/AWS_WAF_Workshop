+++
title = "Dọn dẹp tài nguyên"
date = 2020
weight = 4
chapter = false
pre = "<b>4. </b>"
+++
Bạn sẽ dọn dẹp tài nguyên theo thứ tự sau:

#### Xóa ứng dụng web mẫu
1. Truy cập [CloudFormation Console](https://console.aws.amazon.com/cloudformation/home).
* Chọn **WAFWorkshopSampleWebApp**.
* Click **Delete**.
![Delete the sample web app](/images/4-cleanupresource/cleanupresource-001.png?width=90pc)
2. Click **Delete stack** để xóa.
![Delete the sample web app](/images/4-cleanupresource/cleanupresource-002.png?width=90pc)
#### Xóa Web ACL
1. Truy cập [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start?region=global).
* Click **Web ACLs**.
* Chọn **waf-workshop-juice-shop**.
* Click **Delete**.
![Delete Web ACL](/images/4-cleanupresource/cleanupresource-003.png?width=90pc)
2. Điền ```delete``` để xác nhận, sau đó click **Delete** để xóa.
![Delete Web ACL](/images/4-cleanupresource/cleanupresource-004.png?width=90pc)
#### Xóa Kinesis
1. Truy cập [Amazon Kinesis Console](https://us-east-1.console.aws.amazon.com/kinesis/home?region=us-east-1#/home).
* Click **Delivery streams**.
* Chọn **aws-waf-logs-workshop-26**.
* Click **Delete**.
![Delete Kinesis](/images/4-cleanupresource/cleanupresource-005.png?width=90pc)
2. Điền ```aws-waf-logs-workshop-26``` để xác nhận, sau đó click **Delete** để xóa.
![Delete Kinesis](/images/4-cleanupresource/cleanupresource-006.png?width=90pc)
#### Xóa S3 bucket
1. Truy cập vào [**giao diện quản trị dịch vụ S3**](https://s3.console.aws.amazon.com/s3/).
* Click chọn S3 bucket **aws-waf-logs-001**.
* Click Empty.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-007.png?width=90pc)
2. Điền ```permanently delete``` để xác nhận, sau đó click **Empty** để xóa toàn bộ dữ liệu trong S3 bucket.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-008.png?width=90pc)
* Click **Exit** để trở lại giao diện S3.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-009.png?width=90pc)
3. Click chọn S3 bucket **aws-waf-logs-001** , sau đó click **Delete**.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-010.png?width=90pc)
4. Điền tên bucket sau đó click **Delete bucket** để xóa S3 bucket.
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-011.png?width=90pc)