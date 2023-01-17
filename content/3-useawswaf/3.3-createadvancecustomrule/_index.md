+++
title = "Advanced Custom Rule"
date = 2020
weight = 3
chapter = false
pre = "<b>3.3. </b>"
+++

#### Situation
The milkshake bandits are back attacking your app. They’ve changed their attack again! You’ll need to update the rule to block these requests, whilst allow genuine customers.
#### Create Advanced Custom Rule
{{% notice info %}} 
All WAF Rules are defined as JSON objects. For complex rules, it can be more efficient to work directly with the JSON format than via the Console rules editor. You can retrieve existing rules in JSON format using the API, CLI or Console using the [get-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/get-rule-group.html) command. Modify them using your favourite JSON text editor, then reupload them using [update-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/update-rule-group.html) in the API, CLI or Console.\
Defining rules in JSON allows you to use version control as a source of truth to see how, when and why iterations of complex rulesets have changed.
{{% /notice %}}

For this challenge, you start with a rule:\
This Rule will block any requests that either::
* Contain the header ```x-milkshake: chocolate```
* Contain the query parameter ```milkshake=banana```

1. In the detail of **Web ACL** page.
* Click **Rules**.
* Click **Add Rules**.
* Click **Add my own rules and rule groups**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-001.png?featherlight=false&width=90pc)
2. In the  **Rule builder** section.
* Click **Rule JSON editor**.
* In the **JSON** section, type 
```
{
  "Name": "complex-rule-challenge",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-challenge"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "ByteMatchStatement": {
            "FieldToMatch": {
              "SingleHeader": {
                "Name": "x-milkshake"
              }
            },
            "PositionalConstraint": "EXACTLY",
            "SearchString": "chocolate",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        },
        {
          "ByteMatchStatement": {
            "FieldToMatch": {
              "SingleQueryArgument": {
                "Name": "milkshake"
              }
            },
            "PositionalConstraint": "EXACTLY",
            "SearchString": "banana",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        }
      ]
    }
  }
}
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-002.png?featherlight=false&width=90pc)
3. Drag the screen down, Click **Add rule**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-003.png?featherlight=false&width=90pc)
4. Click **Save**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-004.png?featherlight=false&width=90pc)
#### Update the Rule
This rule was working, but the attackers have adapted. Now malicious requests contain either:
* The header ```x-milkshake: chocolate``` and the header ```x-favourite-topping: nuts```
* The query ```parameter milkshake=banana``` and the query parameter ```favourite-topping=sauce```
1. In the detail of **Web ACL** page.
* Click **Rules**.
* Click **complex-rule-challenge**(the name of the rule you want update).
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-005.png?featherlight=false&width=90pc)
2. Click **Edit**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-006.png?featherlight=false&width=90pc)
3. In the **Rule builder** section.
* Click **Rule JSON editor**.
* In the **JSON** section, type 
```
{
  "Name": "complex-rule-challenge",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-challenge"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "AndStatement": {
            "Statements": [
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleHeader": {
                      "Name": "x-milkshake"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "chocolate",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              },
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleHeader": {
                      "Name": "x-favourite-topping"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "nuts",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "AndStatement": {
            "Statements": [
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleQueryArgument": {
                      "Name": "milkshake"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "banana",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              },
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleQueryArgument": {
                      "Name": "favourite-topping"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "sauce",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-007.png?featherlight=false&width=90pc)
4. Click **Save rule**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-008.png?featherlight=false&width=90pc)
5. Click **Save**
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-009.png?featherlight=false&width=90pc)
6. Run command
```
# This will be allowed
curl -H "x-milkshake: chocolate" "<Your Juice Shop URL>"
curl  "<Your Juice Shop URL>?milkshake=banana"
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-010.png?width=60pc)
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-011.png?width=60pc)
7. Run command
```
# This will be blocked
curl -H "x-milkshake: chocolate" -H "x-favourite-topping: nuts" "<Your Juice Shop URL>"
curl  "<Your Juice Shop URL>?milkshake=banana&favourite-topping=sauce"
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-012.png?width=60pc)
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-013.png?width=60pc)

Blocked requests will give a response like below:
```
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

In this section, you learnt about the JSON format for WAF Rules. Complex logic can be defined in rules using the And, Or and Not operators.