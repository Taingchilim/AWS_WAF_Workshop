+++
title = "ដាក់ឲ្យដំណើរការកម្មវិធីគំរូ"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++

#### ដាក់ឲ្យដំណើរការកម្មវិធីគំរូ

នៅក្នុងការអនុវត្តនេះ អ្នកនឹងប្រើប្រាស់ [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)។ នេះគឺជាកម្មវិធីវេបកូដបើកចំហដែលមិនមានសុវត្ថិភាព។

អ្នកអាចអានឯកសារ [Pwning OWASP Juice Shop](https://pwning.owasp-juice.shop/) ដើម្បីស្វែងយល់បន្ថែមអំពីកម្មវិធី និងចំណុចខ្សោយរបស់វាលម្អិត។

1. ចូលទៅកាន់ **CloudFormation stack** ខាងក្រោម (អាស្រ័យលើ Region) ដើម្បីចាប់ផ្តើមដាក់ឲ្យដំណើរការ។

| Region &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Launch Template                                                                                                                                                                                                                                                                                             |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| US East (N. Virginia) (us-east-1)                                                                                                                                                                                                                               | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-1.s3.us-east-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US East (Ohio) (us-east-2)                                                                                                                                                                                                                                      | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-2.s3.us-east-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US West(Oregon) (us-west-2)                                                                                                                                                                                                                                     | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-west-2.s3.us-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (Ireland) (eu-west-1)                                                                                                                                                                                                                                        | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-1.s3.eu-west-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (London) (eu-west-2)                                                                                                                                                                                                                                         | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-2.s3.eu-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |

2. ចុច **Next**។

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-001.png?featherlight=false&width=90pc)

3. នៅទំព័រ **Specify stack details** ចុច **Next**។

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-002.png?featherlight=false&width=90pc)

4. នៅទំព័រ **Configure stack options** ចុច **Next**។

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-003.png?featherlight=false&width=90pc)

5. នៅទំព័រ **Review WAFWorkshopSampleWebApp**:
   - រុញអេក្រង់ចុះក្រោម។
   - ចុច **I acknowledge that AWS CloudFormation might create IAM resources with custom names**។
   - ចុច **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**។
   - ចុច **Create stack**។

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-004.png?featherlight=false&width=90pc)

**ចំណាំ៖** CloudFormation នឹងត្រូវការប្រហែល 5 នាទី ដើម្បីដាក់ឲ្យដំណើរការកម្មវិធី Juice Shop។ សូមរង់ចាំរហូតដល់ stack ទាំងអស់ស្ថិតក្នុងស្ថានភាព **CREATE_COMPLETE**។

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-005.png?featherlight=false&width=90pc)

6. ចុច **Output** ចុច **dkievcmqb5kzc.cloudfront.net** (អាសយដ្ឋានគេហទំព័រ Juice Shop) ដើម្បីចូលមើលកម្មវិធីរបស់អ្នក។

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-006.png?featherlight=false&width=90pc)
![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-007.png?featherlight=false&width=90pc)