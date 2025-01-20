+++
title = "កត់ត្រាសំណើ"
date = 2020
weight = 5
chapter = false
pre = "<b>3.5. </b>"
+++


#### ស្ថានភាព
ហាងទឹកឃ្មុំរបស់អ្នកកំពុងរីកចម្រើនយ៉ាងឆាប់រហ័ស។ អស្ចារ្យណាស់! ឥឡូវនេះ អ្នកមានបណ្តុំ rule មួយ អ្នកចាប់ផ្តើមមានអារម្មណ៍ថាពិបាកក្នុងការស្គាល់ថា rule មួយណាទទួលខុសត្រូវក្នុងការទប់ស្កាត់សំណើ។ ដូច្នេះ ការកត់ត្រា log ក្លាយជាមានប្រយោជន៍នៅពេលនេះ។ ដើម្បីធ្វើរឿងនេះ អ្នកត្រូវបើកមុខងារកត់ត្រាសម្រាប់ Web ACL របស់អ្នកទៅក្នុង S3 bucket។ Log របស់អ្នកមាន header និង named cookie ដែលរសើប។ អ្នកមិនចង់ឱ្យខ្លឹមសារនេះត្រូវបានកត់ត្រាទេ។ ដូច្នេះ អ្នកត្រូវកំណត់រចនាសម្ព័ន្ធដើម្បីដកចេញ header ក្នុង log។

#### កត់ត្រាសំណើ
{{% notice info %}} 
WAF ប្រើប្រាស់ [Amazon Kinesis Firehose](https://aws.amazon.com/kinesis/data-firehose/) ដើម្បីកត់ត្រា។ នេះអនុញ្ញាតឱ្យកំណត់ត្រាត្រូវបានផ្ទេរទៅកាន់ឃ្លាំង Kinesis Firehose ណាមួយ ដូចជា Amazon S3, Amazon Redshift ឬ Amazon Elastic Search។ ដើម្បីកត់ត្រាសំណើក្នុង Web ACL របស់អ្នក អ្នកត្រូវបង្កើត Kinesis Data Firehose មួយ។
{{% /notice %}}

1. ចូលទៅកាន់ [Amazon Kinesis Console](https://us-east-1.console.aws.amazon.com/kinesis/home?region=us-east-1#/home)។
* ចុច **Kinesis Data Firehose**។
* ចុច **Create delivery stream**។
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-001.png?featherlight=false&width=90pc)

{{% notice note %}} 
អ្នកត្រូវប្រាកដថាបង្កើតនៅក្នុងតំបន់ **us-east-1**។ [នេះជាតម្រូវការចាំបាច់ដើម្បីកត់ត្រា log សម្រាប់ CloudFront](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html)។
{{% /notice %}}

2. នៅក្នុងផ្នែក **Choose source and destination**។
* នៅមុខ **Source** ជ្រើសរើស **Direct PUT**។
* នៅមុខ **Destination** ជ្រើសរើស **Amazon S3**។
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-002.png?featherlight=false&width=90pc)

3. នៅមុខ **Choose source and destination** បំពេញ ```aws-waf-logs-workshop-26```។

{{% notice note %}} 
ឈ្មោះរបស់ Kinesis Data Firehose នឹងចាប់ផ្តើមជាមួយ ```aws-waf-logs-workshop-```។ នេះជាតម្រូវការចាំបាច់របស់សេវាកម្ម WAF។
{{% /notice %}}
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-003.png?featherlight=false&width=90pc)

4. នៅក្នុងផ្នែក **Destination settings**។
* ចុច **Browse**។
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-004.png?featherlight=false&width=90pc)
* ជ្រើសរើស **aws-waf-logs-001**(S3 bucket ដែលបានបង្កើតពីមុន) ជាកន្លែងទុកដាក់គោលដៅ។
* ចុច **Choose**។
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-005.png?featherlight=false&width=90pc)

5. អូសអេក្រង់ចុះក្រោម ចុច **Create delivery stream**។
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-006.png?featherlight=false&width=90pc)

6. ចូលទៅកាន់ [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start)។
* ចុច **Web ACLs**។
* ជ្រើសរើស **Global**។
* ចុច **waf-workshop-juice-shop**។
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-007.png?featherlight=false&width=90pc)

7. នៅទំព័រព័ត៌មាន **Web ACL** របស់អ្នក។
* ជ្រើសរើសថេប **Logging and metrics**។
* ចុច **Enable**។
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-008.png?featherlight=false&width=90pc)

8. នៅក្នុងផ្នែក **Logging destination**។
* ចុច **Kinesis Data Firehose stream**។
* នៅមុខ **Amazon Kinesis Data Firehose delivery stream** ជ្រើសរើស **aws-waf-logs-workshop-26**។
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-009.png?featherlight=false&width=90pc)

9. នៅក្នុងផ្នែក **Redacted fields**។
* ជ្រើសរើស **Single header**។
* នៅមុខ **Redacted headers** ចុច **Add header**។
* បន្ថែមតម្លៃ header ```Cookie```។
* ចុច **Save**។
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-010.png?featherlight=false&width=90pc)

10. រត់ពាក្យបញ្ជា
```
curl "<Your Juice Shop URL>?username=admin"
curl "<Your Juice Shop URL>?milkshake=banana&favourite-topping=sauce"
curl -H "x-milkshake: chocolate" "<Your Juice Shop URL>"
```
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-011.png?width=60pc)
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-012.png?width=60pc)

11. ទាញយកឯកសារ log ដែលបានកត់ត្រាពី S3។
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-013.png?featherlight=false&width=90pc)

12. ស្វែងរកពាក្យគន្លឹះ Cookie ក្នុងឯកសារ។
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-014.png?width=50pc)

#### សេចក្តីសន្និដ្ឋាន
WAF អនុញ្ញាតឱ្យអ្នកកត់ត្រា log សំណើនិងរក្សាទុកនៅក្នុងកន្លែងទុកដាក់គោលដៅរបស់ Kinesis Data Firehose។ Log ផ្តល់ព័ត៌មានអំពីសំណើ។ Log ក៏ផ្តល់ព័ត៌មានអំពីសកម្មភាពនិង rule ដែលបានប៉ះពាល់ដល់សំណើផងដែរ។ ព័ត៌មាននេះអាចមិនមានតម្លៃនៅពេលអ្នកដំណើរការ WAF។ ប្រើប្រាស់មុខងារកែតម្រូវតំបន់ខ្លឹមសារដើម្បីដកចេញព័ត៌មានរសើប។