+++
title = "សាកល្បង Rule ថ្មី"
date = 2020
weight = 4
chapter = false
pre = "<b>3.4. </b>"
+++


មុនពេលដាក់ឱ្យប្រើប្រាស់ rule ថ្មី អ្នកត្រូវធ្វើការសាកល្បងដើម្បីប្រាកដថាអ្នកមិនបានទប់ស្កាត់សំណើត្រឹមត្រូវដោយចៃដន្យ។

នៅក្នុងផ្នែកមុនៗ អ្នកបានប្រើប្រាស់ Block និង Allow នៅពេលកំណត់សកម្មភាពនៅពេលវាយតម្លៃសំណើ។ ក្រៅពីនេះ អ្នកមានជម្រើសមួយទៀតគឺ Count។ Count អនុញ្ញាតឱ្យអ្នកវាយតម្លៃចំនួនសំណើដែលត្រូវនឹងលក្ខខណ្ឌ rule របស់អ្នក។

Count មិនមែនជាសកម្មភាពទប់ស្កាត់ទេ។ នៅពេលសំណើមួយត្រូវនឹង rule ដែលមានសកម្មភាព Count, Web ACL នឹងបន្តដំណើរការ rule ផ្សេងទៀត។

#### ស្ថានភាព
អ្នកបានកំណត់ rule ថ្មីមួយសម្រាប់ WAF របស់អ្នក។ មុនពេលដាក់ឱ្យប្រើប្រាស់វា អ្នកត្រូវសាកល្បងជាមុនសិន។ ការធ្វើបែបនេះគឺដើម្បីកាត់បន្ថយហានិភ័យដែលអ្នកអាចទប់ស្កាត់សំណើត្រឹមត្រូវដោយចៃដន្យ។

Rule ខាងក្រោមនឹងទប់ស្កាត់សំណើដែលមានប៉ារ៉ាម៉ែត្រដែលចូលទៅ username។

#### សាកល្បង Rule ថ្មី
1. បង្កើត rule ថ្មីដូចគ្នានឹង [ការបង្កើត Custom Rule កម្រិតខ្ពស់នៅក្នុងផ្នែក 3.3](../3.3-createadvancecustomrule/)។
* នៅក្នុងផ្នែក **JSON** បំពេញ
```
{
  "Name": "count-von-count",
  "Priority": 0,
  "Action": {
    "Count": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "count-von-count"
  },
  "Statement": {
    "SizeConstraintStatement": {
      "FieldToMatch": {
        "SingleQueryArgument": {
          "Name": "username"
        }
      },
      "ComparisonOperator": "GT",
      "Size": "0",
      "TextTransformations": [
        {
          "Type": "NONE",
          "Priority": 0
        }
      ]
    }
  }
}
```
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-001.png?featherlight=false&width=90pc)
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-002.png?featherlight=false&width=90pc)
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-003.png?featherlight=false&width=90pc)
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-004.png?featherlight=false&width=90pc)

2. រត់ពាក្យបញ្ជា។
```
curl "<Your Juice Shop URL>?username=admin"
```
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-001.png?width=60pc)

3. ចូលទៅកាន់ទំព័រ [CloudWatch Metrics](https://console.aws.amazon.com/cloudwatch/home?#metricsV2:graph=~())។
* ចុច **WAFv2**
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-002.png?featherlight=false&width=90pc)
* ចុច **Rule, WebACL**
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-003.png?featherlight=false&width=90pc)
* ជ្រើសរើស **count-von-count**, យើងនឹងឃើញសំណើ 1 នៅក្នុងផ្នែក **Untitled graph**។
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-004.png?featherlight=false&width=90pc)

មុនពេលដាក់ឱ្យប្រើប្រាស់ rule ថ្មីទៅក្នុង Web ACL របស់អ្នក សូមសាកល្បងវា។ សាកល្បង rule ថ្មីជាមួយសកម្មភាព Count និងតាមដានវាតាមរយៈ CloudWatch metrics។