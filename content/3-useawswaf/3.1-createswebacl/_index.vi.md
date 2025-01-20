+++
title = "Web ACLs ជាមួយ managed rules"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++


#### ស្ថានភាព
សន្មតថាអ្នកជាអ្នកអភិវឌ្ឍន៍ម្នាក់ដែលទើបតែចាប់ផ្តើមជាមួយហាង Juice Shop (ទឹកផ្លែឈើ)។ គេហទំព័ររបស់អ្នកគឺជាកម្មវិធីវេបសាមញ្ញមួយដែលដំណើរការជាមួយមូលដ្ឋានទិន្នន័យ SQL។ ដោយសារហេតុផលមួយចំនួន ក្រុមហេកឃឺ Milkshake (ក្រុមទឹកដោះគោក្រឡុក) កំពុងចាប់ផ្តើមវាយប្រហារគេហទំព័ររបស់អ្នក។

សំណាងល្អ ពេលនេះអ្នកកំពុងចូលដំណើរការ AWS WAF។ ដូច្នេះអ្នកសម្រេចចិត្តដាក់ឲ្យដំណើរការ WAF ដើម្បីការពារគេហទំព័ររបស់អ្នក។

លើសពីនេះទៀត នៅពេលនេះ អ្នកមិនមានពេលច្រើនទេ ដូច្នេះអ្នកនឹងសម្រេចចិត្តប្រើក្រុមច្បាប់ដែលបានបង្កើតឡើងដោយ AWS សម្រាប់ Web ACL របស់អ្នក។ ជាមួយនឹងច្បាប់ទាំងនេះ គេហទំព័ររបស់អ្នកនឹងត្រូវបានការពារពីការវាយប្រហារទូទៅពីក្រុម Milkshake។

#### Web ACLs ជាមួយ managed rules

{{% notice info %}} 
[Web ACLs (Web Access Control List)](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) គឺជាស្នូលនៃការដាក់ឲ្យដំណើរការ AWS WAF។ វារួមមានច្បាប់ដែលត្រូវបានប្រើដើម្បីវាយតម្លៃសំណើដែលត្រូវបានផ្ញើទៅ WAF របស់អ្នក។ Web ACL ត្រូវបានអនុវត្តទៅលើកម្មវិធីវេបរបស់អ្នកតាមរយៈ Amazon CloudFront distribution, AWS API Gateway API ឬ AWS Application Load Balancer។\
[Managed rule groups](https://docs.aws.amazon.com/waf/latest/developerguide/waf-managed-rule-groups.html) គឺជាសំណុំច្បាប់ដែលត្រូវបានបង្កើត និងធ្វើបច្ចុប្បន្នភាពដោយក្រុម AWS ឬភាគីទីបីនៅលើ AWS Marketplace។ ច្បាប់ទាំងនេះផ្តល់នូវសមត្ថភាពការពារកម្មវិធីរបស់អ្នកពីការវាយប្រហារទូទៅ ឬជាក់លាក់សម្រាប់កម្មវិធីនីមួយៗ។
{{% /notice %}}

1. ចូលទៅកាន់ [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start)។
{{% notice note %}} 
ការអនុវត្តនេះប្រើកំណែថ្មីបំផុតនៃ AWS WAF។ សូមប្រាកដថាអ្នកមិនប្រើ WAF Classic ទេ។
{{% /notice %}}
* ចុច **Create web ACL**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-001.png?featherlight=false&width=90pc)
2. ក្នុងផ្នែក **Web ACL details**។
* នៅផ្នែក **Resource type** ចុច **CloudFront distributions**។
* នៅផ្នែក **Name** វាយ ```waf-workshop-juice-shop```។
* នៅផ្នែក **Description** វាយ ```Web ACL for the aws-waf-workshop```។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-002.png?featherlight=false&width=90pc)
3. ក្នុងផ្នែក **Associated AWS resources** ចុច **Add AWS resources**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-003.png?featherlight=false&width=90pc)
4. ក្នុងផ្នែក **Add AWS resources** ចុច **E24BURECS1O10C - dkievcmqb5kzc.cloudfront.net - WAF Workshop CloudFront Distribution** (CloudFront distribution ដែលយើងបានបង្កើត)។
* ចុច **Add**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-004.png?featherlight=false&width=90pc)
5. ចុច **Next**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-005.png?featherlight=false&width=90pc)
6. ក្នុងផ្នែក **Rules**។
* ចុច **Add rules**។
* ចុច **Add managed rule groups**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-006.png?featherlight=false&width=90pc)
7. នៅទំព័រ **Add managed rule groups** ចុច **AWS managed rule groups**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-007.png?featherlight=false&width=90pc)
8. ជ្រើសរើស **Core Rule Set** និង **SQL Database**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-008.png?featherlight=false&width=90pc)
9. រុញអេក្រង់ចុះក្រោម ចុច **Add rules**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-009.png?featherlight=false&width=90pc)
10. នៅទំព័រ **Add managed rule groups** ចុច **Next**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-010.png?featherlight=false&width=90pc)
11. នៅទំព័រ **Set rule priority** ចុច **Next**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-011.png?featherlight=false&width=90pc)
12. នៅទំព័រ **Configure metrics** ចុច **Next**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-012.png?featherlight=false&width=90pc)
13. នៅទំព័រ **Review and create web ACL** រុញអេក្រង់ចុះក្រោម ចុច **Create web ACL**។
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-013.png?featherlight=false&width=90pc)
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-014.png?featherlight=false&width=90pc)
14. រត់ពាក្យបញ្ជា
```
# វាក្លែងធ្វើជាការវាយប្រហារ Cross Site Scripting
# សំណើនេះគួរតែត្រូវបានរារាំង។
curl -X POST  <Your Juice Shop URL> -F "user='<script><alert>Hello></alert></script>'"
```
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-015.png?width=60pc)
15. រត់ពាក្យបញ្ជា
```
# វាក្លែងធ្វើជាការវាយប្រហារ SQL Injection
# សំណើនេះគួរតែត្រូវបានរារាំង។
curl -X POST <Your Juice Shop URL> -F "user='AND 1=1;"
```
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-016.png?width=60pc)