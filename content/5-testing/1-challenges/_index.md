+++
title = "Challenge"
date = 2020
weight = 1
chapter = false
pre = "<b>5.1. </b>"
+++

**Contents:**
- [Challenge](#challenge)
- [Configuration: Update your current Rule and Test](#configuration-update-your-current-rule-and-test)
- [Test case](#test-case)

#### Challenge
You have developed a new rule for your WAF. Before you can deploy it, you must first test it. This is to reduce the risk of unintentionally introducing rules that block genuine requests.

The rule below blocks requests with the query parameter username.

[Read more about testing Web ACLs](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html)

{{%expand "Select to see hints" %}}
1. Update the rule Action of the rule below to Count, so that it can be tested.
```json
{
  "Name": "count-von-count",
  "Priority": 0,
  "Action": {
    "Block": {}
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
2. Deploy the rule to your web ACL, using either to the console or the CLI.
{{% /expand%}}

#### Configuration: Update your current Rule and Test

{{%expand "Select to see steps" %}}
1. Update the rule Action of the rule below to Count

```json
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

![Test Rule](../../../images/5/1.png?width=90pc)

2. Create a new rule in Console, change to JSON editor and paste above rule into the editor.
3. Send few request with query parameter ```username``` to your web application.
4. Navigate to CloudWatch Metrics page. Select **AWS/WAFv2**, then **Region** > **Rule** > **WebACL** to view the CloudWatch Metrics.

![Test Rule](../../../images/5/3.png?width=90pc)

{{% /expand%}}

#### Test case

Execute the following command in your terminal.

```bash
curl "$JUICESHOP_URL?username=admin"
```

![Test Rule](../../../images/5/2.png?width=90pc)

This request won’t be blocked. Instead, it should be counted. Check [CloudWatch metrics](https://console.aws.amazon.com/cloudwatch/home?#metricsV2:graph=~()) to see if it has worked!

![Test Rule](../../../images/5/4.png?width=90pc)
