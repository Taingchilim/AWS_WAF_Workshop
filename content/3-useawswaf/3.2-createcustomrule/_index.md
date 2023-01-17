+++
title = "Custom Rule"
date = 2020
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++
#### Situation
Just as you thought you had solved your milkshake fiasco, more malicious requests are targeting your application. The attacks have become more specific. You realise you can block these attacks with a custom rule for your WAF Web ACL. All of the attacks seem to contain a strange header, X-TomatoAttack. Blocking requests with that header will stop the attack.
#### Tạo Custom Rule
{{% notice info %}} 
WAF allows you to [create your own rules](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) for handling requests. This is useful for adding logic relevant for your specific application. Alongside custom rules, this section will introduce [request sampling](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html#web-acl-testing-view-sample) and [Web ACL Capacity Units](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units).
{{% /notice %}}
1. In the detail of **Web ACL** page.
* Click **Rules**.
* Click **Add Rules**.
* Click **Add my own rules and rule groups**.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-001.png?featherlight=false&width=90pc)
2. In the **Rule builder** section.
* In the **Name** section, type ```MyCustomRule-X-TomatoAttack```.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-002.png?featherlight=false&width=90pc)
3. In the **Statement** section.
* In the **Inspect** section, Select **Single header**.
* In the **Header field name** section, type ```X-TomatoAttack```.
* In the **Match type** section, Select **Size greater than or equal to**.
* In the **Size in bytes** section, type ```0```.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-003.png?featherlight=false&width=90pc)
4. In the **Action** section.
* In the **Action** section, Click **Block**.
* Click **Add rule**.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-004.png?featherlight=false&width=90pc)
5. Click **Save**.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-005.png?featherlight=false&width=90pc)
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-006.png?featherlight=false&width=90pc)
{{% notice info %}} 
You could also achieve the same goal using a [regular expression](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-regex-pattern-set-match).
{{% /notice %}}
6. Run command.
```
# This will be blocked
curl -H "X-TomatoAttack: Red" "<Your Juice Shop URL>"
```
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-007.png?width=60pc)
7. Run command.
```
# This will be blocked
curl -H "X-TomatoAttack: Green" "<Your Juice Shop URL>"
```
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-008.png?width=60pc)
8. In the detail of **Web ACL** page.
* Click **Overview**.
* Drag the screen down, in the **Sampled requests** section, You will see these requests marked as **BLOCK**.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-009.png?featherlight=false&width=90pc)
