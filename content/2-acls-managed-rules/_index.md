+++
title = "Web ACLs and Managed Rules"
date = 2020
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

**Contents:**
- [Web ACLs](#web-acls)
- [Managed Rules](#managed-rules)

#### Web ACLs
A [web ACL (Web Access Control List)](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) is the core resource in an AWS WAF deployment. It contains rules that are evaluated for each request that it receives. A web ACL is associated to your web application via either an Amazon CloudFront distribution, AWS API Gateway API or an AWS Application Load Balancer.

{{% notice tip %}}
This workshop uses the latest version of AWS WAF. Make sure you **do not use** WAF Classic.
{{% /notice %}}

#### Managed Rules
The quickest way to get started with WAF is to deploy an [AWS Managed Rule Group for AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-list.html) to your WebACL.

[Managed Rule Groups](https://docs.aws.amazon.com/waf/latest/developerguide/waf-managed-rule-groups.html) are a set of rules, created and maintained by AWS or third-parties on the AWS Marketplace. These rules provide protections against common types of attacks, or are intended for particular application types.

Each managed rule group protects against a set of common attacks, such as SQL or Command Line attacks.

AWS provide a selection of managed rule groups. Three examples are the *Amazon IP Reputation list*, *Known Bad Inputs* and *Core rule set*.
There are other rule groups available to use
