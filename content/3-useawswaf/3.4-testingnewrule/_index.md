+++
title = "Testing New Rules "
date = 2020
weight = 4
chapter = false
pre = "<b>3.4. </b>"
+++

Before deploying a new rule, it’s vital to test it. This is to ensure you don’t accidentally block valid requests.

So far you have used Block and Allow when specifying what action to take on a request. There is a third action, Count. Count allows you to measure the number of requests that would meet the rule conditions.

Count is a non-terminating action. When a request matches a rule with the Count action, the web ACL will continue processing the remaining rules.

#### Situation
You have developed a new rule for your WAF. Before you can deploy it, you must first test it. This is to reduce the risk of unintentionally introducing rules that block genuine requests.

The rule below blocks requests with the query parameter username.
#### Kiểm thử Rule mới
1. Create a new rule like [Create Advanced Custom Rule in 3.3 section](../3.3-createadvancecustomrule/).

* In the **JSON** section, type 
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

2. Run command
```
curl "<Your Juice Shop URL>?username=admin"
```
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-001.png?width=60pc)
3. Go to [CloudWatch Metrics](https://console.aws.amazon.com/cloudwatch/home?#metricsV2:graph=~()).
* Click **WAFv2**
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-002.png?featherlight=false&width=90pc)
* Click **Rule, WebACL**
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-003.png?featherlight=false&width=90pc)
* Select **count-von-count**, We will see a request in **Untitled graph** section.
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-004.png?featherlight=false&width=90pc)

Before deploying a new rule, it’s vital to test it. This is to ensure you don’t accidentally block valid requests.