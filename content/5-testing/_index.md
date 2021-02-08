+++
title = "Testing New Rules"
date = 2020
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

Before deploying a new rule, it’s vital to test it. This is to ensure you don’t accidentally block valid requests.

So far you have used *Block* and *Allow* when specifying what action to take on a request. There is a third action, *Count*. *Count* allows you to measure the number of requests that would meet the rule conditions.

Count is a *non-terminating* action. When a request matches a rule with the *Count* action, the web ACL will continue processing the remaining rules.

{{% notice tip %}}
Managed rules and rule groups can also be tested in a similar way using *Count*.
{{% /notice %}}

**Contents**
- [Viewing rule counts](#viewing-rule-counts)

#### Viewing rule counts
When a rule with action *Count* is matched, the event is emitted as CloudWatch metrics. To view the count for a rule, navigate to the [CloudWatch metrics console](https://console.aws.amazon.com/cloudwatch/home?#metricsV2:graph=~()). Select *AWS/WAFv2*, then *Region*, *Rule*, *WebACL* to view you metrics.

{{% notice tip %}}
By default, *Average* is used when displaying WAF metrics. It’s useful to change this to *Sum* in some scenarios.
{{% /notice %}}
