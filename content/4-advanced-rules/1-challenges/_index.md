+++
title = "Advanced Rule Challenge"
date = 2020
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++

**Contents:**
- [Challenge](#challenge)
- [Update JSON Rule](#update-json-rule)
- [Test Case](#test-case)

#### Challenge
The milkshake bandits are back attacking your workshop. They’ve changed their attack again! You’ll need to create a new rule to block these requests, whilst allow genuine customers For this challenge, you start with an existing rule.

{{%expand "Select to see your starting rule" %}}
```json
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
{{% /expand%}}

Currently it will block any requests that either:

1. Contain the *header* ```x-milkshake: chocolate```
2. Contain the *query parameter* ```milkshake=banana```

This rule was working, but the attackers have adapted. Now malicious requests contain either

1. The *header* ```x-milkshake: chocolate``` AND the *header* ```x-favourite-topping: nuts```
2. The *query parameter* ```milkshake=banana``` AND the *query parameter* ```favourite-topping=sauce```

Update the existing rule. Use *AndStatement* to extend the two existing statements. *AndStatement* has the following syntax:

```json
"AndStatement": {
  "Statements": [
    # Add your statements here
  ]
}
```

#### Update JSON Rule

{{%expand "Select to see the JSON rule" %}}
```json
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

![Advanced Rule](../../../images/4/1.png?width=90pc)

![Advanced Rule](../../../images/4/2.png?width=90pc)
{{% /expand%}}

#### Test Case

Test your new rule by creating the following requests using *curl*.

```bash
# Set the JUICESHOP_URL if not already done
JUICESHOP_URL=<Your Juice Shop URL>
```

```bash
# Allowed
curl -H "x-milkshake: chocolate" "${JUICESHOP_URL}"
curl  "${JUICESHOP_URL}?milkshake=banana"
```

![Advanced Rule](../../../images/4/3.png?width=90pc)

![Advanced Rule](../../../images/4/4.png?width=90pc)

```bash
# Blocked
curl -H "x-milkshake: chocolate" -H "x-favourite-topping: nuts" "${JUICESHOP_URL}"
curl  "${JUICESHOP_URL}?milkshake=banana&favourite-topping=sauce"
```

![Advanced Rule](../../../images/4/5.png?width=90pc)

![Advanced Rule](../../../images/4/6.png?width=90pc)

Blocked requests will give a response like below

```bash
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