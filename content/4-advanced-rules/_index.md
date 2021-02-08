+++
title = "Advanced Custom Rules"
date = 2020
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

You’ve created a simple WAF rule that evaluates one part of a request. How could you create a rule to evaulate multiple parts of a request?

**Contents:**
- [Rules in JSON](#rules-in-json)
- [Boolean Logic in Rules](#boolean-logic-in-rules)
- [Example](#example)

#### Rules in JSON
All WAF Rules are defined as JSON objects. For complex rules, it can be more efficient to work directly with the JSON format than via the Console rules editor. You can retrieve existing rules in JSON format using the API, CLI or Console using the [get-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/get-rule-group.html) command. Modify them using your favourite JSON text editor, then reupload them using [update-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/update-rule-group.html) in the API, CLI or Console.

Defining rules in JSON allows you to use version control as a source of truth to see how, when and why iterations of complex rulesets have changed.

{{% notice tip %}}
The syntax for defining rules in JSON is [provided in the update-rule-group documentation](https://docs.aws.amazon.com/cli/latest/reference/wafv2/update-rule-group.html)
{{% /notice %}}

If you’re ever unsure of the syntax, it can be helpful to create a simple example in the Console visual editor first, then switch to the JSON editor to see the JSON equivalent.

#### Boolean Logic in Rules

The ```AND```, ```OR``` and ```NOT``` operators can be used to create more complex rules. This is useful to inspect multiple parts of a request. For example, you could only allow a request if the query string OR the Header contains a certain key/value.

{{% notice tip %}}
Nested rules can be created using the visual editor. However, they are limited to one level deep.
To create arbitarily nested rules in the console, the JSON editor must be used.
Use the validate action in the console JSON editor to validate the rule
{{% /notice %}}

Below is an example of a rule created in the console. This rule will block requests with a query string of length greater than or equal to 0.

{{% notice info %}}
This rule will block any request containing a query string.
{{% /notice %}}

Here is the same rule defined in JSON

- Action specifies the action taken by WAF if the rule evaluates to true.
- VisibilityConfig is used to configure request sampling and CloudWatch metrics.
- Statement defines the rule expression to be evaluated.

```json
{
  "Name": "example-rule-01",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "example-rule-01"
  },
  "Statement": {
    "SizeConstraintStatement": {
      "FieldToMatch": {
        "QueryString": {}
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

#### Example

You’ve been asked by your favourite colleague for some help. They are recieving an attack. They need to block malicious incoming requests without blocking requests from actual customers. The malicious requests contain a body over 100kb, but are missing a header, x-upload-photo: true.

You quickly realise that this isn’t possible to express using the console rule editor. You will need to edit some JSON.

Let’s start with an empty rule:

```json
{
  "Name": "example-rule-02",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "example-rule-02"
  },
  "Statement": {
    // We will add the rule here
  }
}
```

There are two cases we need to consider

1. If the request body is larger than 100kb, block the request
2. If the request does not contain a header of ```x-upload-body: true```, block the request

We will need to use the [OrStatement](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-or.html) and [NotStatement](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-not.html) to express this logic.

```json
{
// fields omitted for brevity
"Statement": {
  "OrStatement": {
    "Statements": [
      {
        // Inspect Body Size here
      },
      {
        "NotStatement": {
           // Inspect Header here
        }
      }
    ]
  }
}
```

To inspect the size of the body, we will use the SizeConstraintStatement to validate the size of the request body.

```json
"SizeConstraintStatement": {
  "FieldToMatch": {
    "Body": {}
  },
  "ComparisonOperator": "GT",
  "Size": "100",
  "TextTransformations": [
    {
      "Type": "NONE",
      "Priority": 0
    }
  ]
}

```

To inspect the Headers of the request, use the ByteMatchStatement

```json
"ByteMatchStatement": {
  "FieldToMatch": {
    "SingleHeader": {
      "Name": "x-upload-image"
    }
  },
  "PositionalConstraint": "EXACTLY",
  "SearchString": "true",
  "TextTransformations": [
    {
      "Type": "NONE",
      "Priority": 0
    }
  ]
}
```

Here is the final rule

```json
{
  "Name": "complex-rule-example",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-example"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "SizeConstraintStatement": {
            "FieldToMatch": {
              "Body": {}
            },
            "ComparisonOperator": "GT",
            "Size": "100",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        },
        {
          "NotStatement": {
            "Statement": {
              "ByteMatchStatement": {
                "FieldToMatch": {
                  "SingleHeader": {
                    "Name": "x-upload-body"
                  }
                },
                "PositionalConstraint": "EXACTLY",
                "SearchString": "true",
                "TextTransformations": [
                  {
                    "Type": "NONE",
                    "Priority": 0
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}
```