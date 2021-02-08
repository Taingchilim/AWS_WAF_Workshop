+++
title = "Custom Rules"
date = 2020
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

**Contents:**
- [Custom Rule](#custom-rule)
- [Create Custom Rule](#create-custom-rule)
- [Request Sampling](#request-sampling)
- [Web ACL Capacity Units](#web-acl-capacity-units)

#### Custom Rule
WAF allows you to [create your own rules](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) for handling requests. This is useful for adding logic relevant for your specific application. Alongside custom rules, this section will introduce [request sampling](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html#web-acl-testing-view-sample) and [Web ACL Capacity Units](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units).

#### Create Custom Rule
The simplest way to create a custom rule is to use the Editor in the WAF Console.

![Custom Rule](../../../images/3/1.png?width=90pc)

[Rules](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) allow you to inspect [components of a the HTTP request](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-fields.html) such as:

- Source IP
- Headers
- Body
- URI
- Query Parameters

Based on the component inspected, you can block or allow a request.

#### Request Sampling
WAF allows you to view a sample of requests that it has processed. Do this from your Web ACL dashboard

![Request Sampling](../../../images/3/2.png?width=90pc)

This is useful for quick debugging to see what requests have been received and how they were handled.

It’s also possible to log all requests your Web ACL receives. This will be introduced later

#### [Web ACL Capacity Units](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units)

{{% notice tip %}}
Your web ACL has a maxiumum WCU of 1500. This can be increased by contacting AWS Support.
{{% /notice %}}

[More details about WCU.](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units)