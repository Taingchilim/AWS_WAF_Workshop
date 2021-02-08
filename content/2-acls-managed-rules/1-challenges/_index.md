+++
title = "Web ACLs and Managed Rules"
date = 2020
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

**Contents:**
- [Challenges](#challenges)
- [Configuration: Create Web ACL](#configuration-create-web-acl)
- [Configuration: Adding Managed Rule Group](#configuration-adding-managed-rule-group)
- [Test Case](#test-case)

#### Challenges
You are the sole developer for the start up Juice Shop. Your website is a simple web application backed by a SQL Database. For some reason, a group of Milkshake bandits have started attacking your site!

Luckily, you recently attended a workshop on AWS WAF. You decide to implement your own WAF to protect your site.

At this time, you don’t have much time, so you decide to deploy two AWS Managed Rule groups to your WebACL. This will protect your website from the common attacks the milkshake bandits are using.

#### Configuration: Create Web ACL

**Create a web ACL in the WAF console.**

{{%expand "Select to see the steps" %}}
1. Navigate to the [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start).
2. Select **Create Web ACL**.
3. Set the **Region** to the **Global (CloudFront)**.
   - Set the name to ```waf-workshop-juice-shop```.
   - Set the description as ```web ACL for the aws-waf-workshop```.
   - Leave the **Resource type** as **CloudFront Distribution**.

![CloudFormation](../../../images/2/1.png?width=90pc)

4. In the **Associated AWS resources** section, select **Add AWS resources**.

![CloudFormation](../../../images/2/2.png?width=90pc)

5. Select the **CloudFront distribution**.

![CloudFormation](../../../images/2/3.png?width=90pc)
{{% /expand%}}

**Associate the web ACL with the CloudFront distribution for your site.**

{{%expand "Select to see notices" %}}
If the CloudFront distribution does not appear when associating it to the web ACL, double check that:
1. The web ACL **Resource type** is set to **CloudFront Distribution**.
2. The CloudFormation template in [Part 1 - Prerequiste](../../1-prerequiste/#deploy-the-sample-web-app) has been deployed successfully.
{{% /expand%}}

#### Configuration: Adding Managed Rule Group

Add two managed rule groups to your WebACL.

{{%expand "Select to see the steps" %}}
1. Navigate to the **Rules** tab of your **web ACL**.
2. Select **Add Rules** > **Add Managed Rule Groups**.

![CloudFormation](../../../images/2/4.png?width=90pc)

3. Select **Core Rule Set** and **SQL Database** from the AWS managed rule groups.

![CloudFormation](../../../images/2/5.png?width=90pc)

![CloudFormation](../../../images/2/6.png?width=90pc)

4. Select **Add rules** to add rule to your Web ACL.
5. Select **Next** in **Step 3** and **Step 4**. Finally, select **Create web ACL** in **Step 5** to create Web ACL.

![CloudFormation](../../../images/2/7.png?width=90pc)

![CloudFormation](../../../images/2/8.png?width=90pc)
{{% /expand%}}

#### Test Case

Test your new rules with the commands below.
Make sure the ```JUICESHOP_URL``` variable to contains the URL for your Juice Shop deployment.

```bash
export JUICESHOP_URL=<Your Juice Shop URL>
```

```bash
# This imitates a Cross Site Scripting attack
# This request should be blocked.
curl -X POST  $JUICESHOP_URL -F "user='<script><alert>Hello></alert></script>'"
```

![CloudFormation](../../../images/2/9.png?width=90pc)

```bash
# This imitates a SQL Injection attack
# This request should be blocked.
curl -X POST $JUICESHOP_URL -F "user='AND 1=1;"
```

![CloudFormation](../../../images/2/10.png?width=90pc)

- If a request is blocked, you will receive a HTML response stating the request was forbidden. Here’s an snippet of what to expect

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

- If you receive a response like below, then the request wasn’t blocked by WAF.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>OWASP Juice Shop</title>
    // Omitted for brevity
  </head>
</html>
```

In this case, something has gone wrong. Double check the following:

- Is your web ACL associated with the CloudFront distribution?
  - The CloudFront distribution will not be protected if the web ACL is not associated.
- Does your web ACL contain two active rules, the Core Rule Set and Sql database?
  - If the managed rules are not active, then no rules will exist in the web ACL to block the request.