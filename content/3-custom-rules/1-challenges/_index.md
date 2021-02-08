+++
title = "ACLs and Rules Challenge"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++

**Contents:**
- [Challenge](#challenge)
- [Configuration: Create custom rule](#configuration-create-custom-rule)
- [Test Case](#test-case)

#### Challenge
Just as you thought you had solved your milkshake fiasco, more malicious requests are targeting your application. The attacks have become more specific. You realise you can block these attacks with a custom rule for your WAF Web ACL.
All of the attacks seem to contain a strange header, ```X-TomatoAttack```. Blocking requests with that header will stop the attack

{{%expand "Select to see a hint" %}}
- Create a new custom rule
- The malicious requests contain a specific header.
- How long is the value of this header?
{{% /expand%}}

#### Configuration: Create custom rule

Create a rule on your Web ACL that blocks requests with the header ```X-TomatoAttack``` with ANY value.

{{%expand "Select to see the steps" %}}
1. Add a **Custom Rule** to your **Web ACL**

![ACLs and Rules Challenge](../../../images/3/1.png?width=90pc)

2. Inspect the **Header** of the request
3. Add a **Custom Rule** to your Web ACL
4. Inspect the ```Header``` of the request
5. If the Header ```X-TomatoAttack``` >= 0, block the request.

![ACLs and Rules Challenge](../../../images/3/2.png?width=90pc)

![ACLs and Rules Challenge](../../../images/3/3.png?width=90pc)

![ACLs and Rules Challenge](../../../images/3/4.png?width=90pc)
{{% /expand%}}

You could also achieve the same goal using a [regular expression](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-regex-pattern-set-match.html).

If you rule is not working, double check:

1. The web ACL is associated with the Application Load Balancer
2. The header field name is spelt correctly

#### Test Case

This test case will send a request your test application. If the WAF rule is working, your request should be blocked. You will receive a *403* response like below

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
    <title>ERROR: The request could not be satisfied</title>
  </head>
  <body>
    <h1>403 ERROR</h1>
    <!-- Omitted -->
  </body>
</html>
```

Run the command below in your terminal.

```bash
# Set the JUICESHOP_URL variable if not already done
# JUICESHOP_URL=<Your Juice Shop URL>

# This should be blocked
curl -H "X-TomatoAttack: Red" "${JUICESHOP_URL}"
```

![ACLs and Rules Challenge](../../../images/3/5.png?width=90pc)

```bash
# This should be blocked
curl -H "X-TomatoAttack: Green" "${JUICESHOP_URL}"
```

![ACLs and Rules Challenge](../../../images/3/6.png?width=90pc)

Check your [WebACL overview to see the sampled requests](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html#web-acl-testing-view-sample#web-acl-testing-view-sample). You should see these requests marked as **BLOCK**.

![ACLs and Rules Challenge](../../../images/3/7.png?width=90pc)

Phew, it seems like your custom rule worked.

Custom rules allow you to implement your own logic for handling requests in WAF. Custom rules can inspect many components of a request, then act to block or allow a request if the rule statement is true.

Every Web ACL has a maxiumum Web ACL Capacity Units (WCU). This is 1500, but can be increased if needed. Every rule and rule group in a Web ACL contributes towards this limit.
