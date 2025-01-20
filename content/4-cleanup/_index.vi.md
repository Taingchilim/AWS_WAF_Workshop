
+++
title = "សម្អាតធនធាន"
date = 2020
weight = 4
chapter = false
pre = "<b>4. </b>"
+++
អ្នកនឹងសម្អាតធនធានតាមលំដាប់ដូចខាងក្រោម៖

#### លុបកម្មវិធីគំរូនៅលើវ៉ិប
1. ចូលទៅកាន់ [CloudFormation Console](https://console.aws.amazon.com/cloudformation/home)។
* ជ្រើសរើស **WAFWorkshopSampleWebApp**។
* ចុច **Delete**។
![Delete the sample web app](/images/4-cleanupresource/cleanupresource-001.png?featherlight=false&width=90pc)

2. ចុច **Delete stack** ដើម្បីលុប។
![Delete the sample web app](/images/4-cleanupresource/cleanupresource-002.png?featherlight=false&width=90pc)

#### លុប Web ACL
1. ចូលទៅកាន់ [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start?region=global)។
* ចុច **Web ACLs**។
* ជ្រើសរើស **waf-workshop-juice-shop**។
* ចុច **Delete**។
![Delete Web ACL](/images/4-cleanupresource/cleanupresource-003.png?featherlight=false&width=90pc)

2. បំពេញ ```delete``` ដើម្បីបញ្ជាក់ បន្ទាប់មកចុច **Delete** ដើម្បីលុប។
![Delete Web ACL](/images/4-cleanupresource/cleanupresource-004.png?featherlight=false&width=90pc)

#### លុប Kinesis
1. ចូលទៅកាន់ [Amazon Kinesis Console](https://us-east-1.console.aws.amazon.com/kinesis/home?region=us-east-1#/home)។
* ចុច **Delivery streams**។
* ជ្រើសរើស **aws-waf-logs-workshop-26**។
* ចុច **Delete**។
![Delete Kinesis](/images/4-cleanupresource/cleanupresource-005.png?featherlight=false&width=90pc)

2. បំពេញ ```aws-waf-logs-workshop-26``` ដើម្បីបញ្ជាក់ បន្ទាប់មកចុច **Delete** ដើម្បីលុប។
![Delete Kinesis](/images/4-cleanupresource/cleanupresource-006.png?featherlight=false&width=90pc)

#### លុប S3 bucket
1. ចូលទៅកាន់ [**ផ្ទាំងគ្រប់គ្រងសេវាកម្ម S3**](https://s3.console.aws.amazon.com/s3/)។
* ចុចជ្រើសរើស S3 bucket **aws-waf-logs-001**។
* ចុច Empty។
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-007.png?featherlight=false&width=90pc)

2. បំពេញ ```permanently delete``` ដើម្បីបញ្ជាក់ បន្ទាប់មកចុច **Empty** ដើម្បីលុបទិន្នន័យទាំងអស់ក្នុង S3 bucket។
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-008.png?featherlight=false&width=90pc)
* ចុច **Exit** ដើម្បីត្រឡប់ទៅផ្ទាំង S3 វិញ។
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-009.png?featherlight=false&width=90pc)

3. ចុចជ្រើសរើស S3 bucket **aws-waf-logs-001** បន្ទាប់មកចុច **Delete**។
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-010.png?featherlight=false&width=90pc)

4. បំពេញឈ្មោះ bucket បន្ទាប់មកចុច **Delete bucket** ដើម្បីលុប S3 bucket។
![Delete S3 bucket](/images/4-cleanupresource/cleanupresource-011.png?featherlight=false&width=90pc)