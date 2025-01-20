+++
title = "Custom Rule"
date = 2020
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++
#### ស្ថានភាព
ភ្លាមៗនោះ នៅពេលអ្នកគិតថាអ្នកបានរារាំងការវាយប្រហារពីក្រុម Milkshake មានសំណើវាយប្រហារបន្ថែមទៀតចូលមកកម្មវិធីរបស់អ្នក។ ការវាយប្រហារទាំងនេះកាន់តែគ្រោះថ្នាក់ជាងមុន។ ទោះយ៉ាងណា អ្នកអាចមានទំនុកចិត្តថាអាចរារាំងការវាយប្រហារទាំងនេះដោយប្រើច្បាប់ផ្ទាល់ខ្លួនសម្រាប់ WAF Web ACL របស់អ្នក។

ការវាយប្រហារទាំងអស់ហាក់ដូចជាមាន header ចម្លែក ```X-TomatoAttack``` និងអ្នកត្រូវរារាំងសំណើដែលមាន header នេះដើម្បីរារាំងការវាយប្រហារនេះ។

#### បង្កើត Custom Rule
{{% notice info %}} 
WAF អនុញ្ញាតឱ្យអ្នកបង្កើត [ច្បាប់ផ្ទាល់ខ្លួន](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) ដើម្បីដោះស្រាយសំណើ។ នេះមានប្រយោជន៍ខ្លាំងណាស់នៅពេលអ្នកចង់កែសម្រួលតាមបរិបទកម្មវិធីរបស់អ្នក។ រួមជាមួយនឹងការណែនាំអំពីច្បាប់ផ្ទាល់ខ្លួន ផ្នែកនេះអ្នកនឹងស្គាល់ជាមួយនឹងការ [បង្កើតគំរូសំណើ](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html#web-acl-testing-view-sample) និង [Web ACL Capacity Units](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units)។
{{% /notice %}}

1. នៅទំព័រព័ត៌មាន **Web ACL** របស់អ្នក។
* ចុច **Rules**។
* ចុច **Add Rules**។
* ចុច **Add my own rules and rule groups**។
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-001.png?featherlight=false&width=90pc)

2. ក្នុងផ្នែក **Rule builder**។
* នៅផ្នែក **Name** វាយ ```MyCustomRule-X-TomatoAttack```។
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-002.png?featherlight=false&width=90pc)

3. ក្នុងផ្នែក **Statement**។
* នៅផ្នែក **Inspect** ជ្រើសរើស **Single header**។
* នៅផ្នែក **Header field name** វាយ ```X-TomatoAttack```។
* នៅផ្នែក **Match type** ជ្រើសរើស **Size greater than or equal to**។
* នៅផ្នែក **Size in bytes** វាយ ```0```។
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-003.png?featherlight=false&width=90pc)

4. ក្នុងផ្នែក **Action**។
* នៅផ្នែក **Action** ចុច **Block**។
* ចុច **Add rule**។
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-004.png?featherlight=false&width=90pc)

5. ចុច **Save**។
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-005.png?featherlight=false&width=90pc)
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-006.png?featherlight=false&width=90pc)

{{% notice info %}} 
អ្នកអាចទទួលបានលទ្ធផលដូចគ្នាប្រសិនបើប្រើ [regular expression](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-regex-pattern-set-match.html)
{{% /notice %}}

6. រត់ពាក្យបញ្ជា។
```
# នេះនឹងត្រូវបានរារាំង
curl -H "X-TomatoAttack: Red" "<Your Juice Shop URL>"
```
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-007.png?width=60pc)

7. រត់ពាក្យបញ្ជា។
```
# នេះនឹងត្រូវបានរារាំង
curl -H "X-TomatoAttack: Green" "<Your Juice Shop URL>"
```
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-008.png?width=60pc)

8. នៅទំព័រព័ត៌មាន **Web ACL** របស់អ្នក។
* ចុច **Overview**។
* រុញអេក្រង់ចុះក្រោមទៅផ្នែក **Sampled requests** អ្នកនឹងឃើញសំណើដែលទទួលបានមានស្ថានភាព **BLOCK**។
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-009.png?featherlight=false&width=90pc)