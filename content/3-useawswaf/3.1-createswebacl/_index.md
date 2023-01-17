+++
title = "Web ACLs with managed rules"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1.</b>"
+++
#### Situation
You are the sole developer for the start up Juice Shop. Your website is a simple web application backed by a SQL Database. For some reason, a group of Milkshake bandits have started attacking your site!

Luckily, you recently attended a workshop on AWS WAF. You decide to implement your own WAF to protect your site.

At this time, you don’t have much time, so you decide to deploy two AWS Managed Rule groups to your WebACL. This will protect your website from the common attacks the milkshake bandits are using.
#### Web ACLs with managed rules

{{% notice info %}} 
[Web ACLs (Web Access Control List)](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) is the core resource in an AWS WAF deployment. It contains rules that are evaluated for each request that it receives. A web ACL is associated to your web application via either an Amazon CloudFront distribution, AWS API Gateway API or an AWS Application Load Balancer.\
[Managed rule groups](https://docs.aws.amazon.com/waf/latest/developerguide/waf-managed-rule-groups.html) are a set of rules, created and maintained by AWS or third-parties on the AWS Marketplace. These rules provide protections against common types of attacks, or are intended for particular application types.
{{% /notice %}}

1. Goto [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start).
{{% notice note %}} 
This workshop uses the latest version of AWS WAF. Make sure you do not use WAF Classic.
{{% /notice %}}
* Click **Create web ACL**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-001.png?featherlight=false&width=90pc)
2. In the **Web ACL details** section.
* In the **Resource type** section, Click **CloudFront distributions**.
* In the **Name** section type ```waf-workshop-juice-shop```.
* In the **Description** section type ```Web ACL for the aws-waf-workshop```.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-002.png?featherlight=false&width=90pc)
3. In the **Associated AWS resources** section, Click **Add AWS resources**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-003.png?featherlight=false&width=90pc)
4. In the **Add AWS resources** section, Click **E24BURECS1O10C - dkievcmqb5kzc.cloudfront.net - WAF Workshop CloudFront Distribution**(CloudFront distribution we created).
* Click **Add**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-004.png?featherlight=false&width=90pc)
5. Click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-005.png?featherlight=false&width=90pc)
6. In the **Rules** section.
* Click **Add rules**.
* Click **Add managed rule groups**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-006.png?featherlight=false&width=90pc)
7. In the **Add managed rule groups** page, Click **AWS managed rule groups**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-007.png?featherlight=false&width=90pc)
8. Select **Core Rule Set** and **SQL Database**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-008.png?featherlight=false&width=90pc)
9. Drag the screen down, Click **Add rules**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-009.png?featherlight=false&width=90pc)
10. In the **Add managed rule groups** page, click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-010.png?featherlight=false&width=90pc)
11. In the **Set rule priority** page, click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-011.png?featherlight=false&width=90pc)
12. In the **Configure metrics** page, click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-012.png?featherlight=false&width=90pc)
13. In the **Review and create web ACL** page, Drag the screen down, click **Create web ACL**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-013.png?featherlight=false&width=90pc)
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-014.png?featherlight=false&width=90pc)
14. Run command
```
# This imitates a Cross Site Scripting attack
# This request should be blocked.
curl -X POST  <Your Juice Shop URL> -F "user='<script><alert>Hello></alert></script>'"
```
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-015.png?width=60pc)
15. Run command
```
# This imitates a SQL Injection attack
# This request should be blocked.
curl -X POST <Your Juice Shop URL> -F "user='AND 1=1;"
```
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-016.png?width=60pc)

