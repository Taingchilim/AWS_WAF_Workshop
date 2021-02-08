+++
title = "Logging Challenge"
date = 2020
weight = 1
chapter = false
pre = "<b>6.1. </b>"
+++

**Contents:**
- [Challenge](#challenge)
- [Configuration: Logging](#configuration-logging)
- [Conclusion](#conclusion)

#### Challenge
> The Juice Shop is growing rapidly. Great work! Now that you have a set of rules, it is becoming more difficult to reason which rule is responsible for blocking a request. It would be helpful to have some logs. To do this, you need to enable logging for your Web ACL to an S3 Bucket.
> Your logs contain a sensitive header, named Cookie. You don’t want this to be stored in your logs. You will need configure the redaction of this header in the logs.

{{% notice tip %}}
Use [Logging Web ACL Traffic Information](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html) to help you
{{% /notice %}}

1. Create an S3 bucket. This will be the destination of your Kinesis Data Firehose
   - Prefix the S3 bucket with ```aws-waf-logs-workshop-```. This is to help you find it later.

![Logging](../../../images/6/1.png?width=90pc)

{{% notice info %}}
When using a Kinesis Data Firehose to ingest WAF requests, the Firehose name must have the prefix ```aws-waf-logs-```
{{% /notice %}}

2. Create a Kinesis Data Firehose delivery stream. Make sure to create the resource in ```us-east-1```. [This is required when capturing logs for CloudFront](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html). 
   - Prefix the Kinesis Data Firehose with ```aws-waf-logs-workshop-```. This is required by the WAF service
   - Use the S3 bucket from the previous step as your destination.
3. Enable logging for your WAF
4. Redact the cookie, titled ```Cookie``` (the Juice Shop Cookie) in your logs.
5. Generate some traffic using the following requests. Some of these will be blocked by the rules you’ve previously created
```bash
curl "$JUICESHOP_URL?username=admin"
curl "${JUICESHOP_URL}?milkshake=banana&favourite-topping=sauce"
curl -H "x-milkshake: chocolate" "${JUICESHOP_URL}"
```
6. Download the log file from the S3 bucket used as the Kinesis Data Firehose destination.
7. Inspect the logs. Can you find the redacted ```Cookie``` field?
{{% notice tip %}}
View the redacted fields for a logging configuration with the [*get-logging-configuration*](https://docs.aws.amazon.com/cli/latest/reference/wafv2/get-logging-configuration.html) CLI command
{{% /notice %}}

#### Configuration: Logging

{{%expand "Select to see the answer" %}}
1. Create a Kinesis Data Firehose in ```us-east-1```. Make sure the name begins with ```aws-waf-logs-```. For example, ```aws-waf-logs-workshop```. Select S3 as the Firehose destination.

![Logging](../../../images/6/2.png?width=90pc)

![Logging](../../../images/6/3.png?width=90pc)

![Logging](../../../images/6/4.png?width=90pc)

![Logging](../../../images/6/5.png?width=90pc)

![Logging](../../../images/6/6.png?width=90pc)

![Logging](../../../images/6/7.png?width=90pc)

![Logging](../../../images/6/8.png?width=90pc)

2. Select your Web ACL in the WAF Console
3. Select the **Logging and Metrics** Tab
4. Select **Enable logging**

![Logging](../../../images/6/9.png?width=90pc)

5. Select the Kinesis Date Firehose you wish to use.
6. Under **Redacted Fields**, tick **Header**. Add the Header value ```Cookie```

![Logging](../../../images/6/10.png?width=90pc)

![Logging](../../../images/6/11.png?width=90pc)

7. Run the ```curl``` commands in the challenge above to generate traffic

![Logging](../../../images/6/12.png?width=90pc)

8. Download the log files from S3.

![Logging](../../../images/6/13.png?width=90pc)

9. Search for the ```Cookie``` header in the log file

![Logging](../../../images/6/14.png?width=90pc)
{{% /expand%}}

#### Conclusion
WAF allows you to capture request logs and store them in any Kinesis Data Firehose destination. The logs provide information of the request. The logs also provide the action and rule involved for a request. This information can be invaluable when running a WAF. Use field redaction to avoid logging sensitive information.