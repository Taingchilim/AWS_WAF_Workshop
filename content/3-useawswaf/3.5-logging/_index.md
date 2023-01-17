+++
title = "Log the requests"
date = 2020
weight = 5
chapter = false
pre = "<b>3.5. </b>"
+++

#### Situation
The Juice Shop is growing rapidly. Great work! Now that you have a set of rules, it is becoming more difficult to reason which rule is responsible for blocking a request. It would be helpful to have some logs. To do this, you need to enable logging for your Web ACL to an S3 Bucket. Your logs contain a sensitive header, named Cookie. You don’t want this to be stored in your logs. You will need configure the redaction of this header in the logs.

#### Log the requests
{{% notice info %}} 
WAF Uses [Amazon Kinesis Firehose](https://aws.amazon.com/kinesis/data-firehose/) to ingest logs. This allows logs to be passed to any Kinesis Firehose destination, such as Amazon S3, Amazon Redshift or Amazon Elastic Search. To enable logging of requests in your Web ACL, you must first create a Kinesis Data Firehose.
{{% /notice %}}
1. Go to [Amazon Kinesis Console](https://us-east-1.console.aws.amazon.com/kinesis/home?region=us-east-1#/home).
* Click **Kinesis Data Firehose**.
* Click **Create delivery stream**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-001.png?featherlight=false&width=90pc)
{{% notice note %}} 
Make sure to create the resource in **us-east-1**. [This is required when capturing logs for CloudFront](https://docs.aws.amazon.com/waf/latest/developerguide/logging.
{{% /notice %}}
2. In the **Choose source and destination** section.
* In the **Source** section, select **Direct PUT**.
* In the **Destination** section, select **Amazon S3**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-002.png?featherlight=false&width=90pc)
3. In the **Choose source and destination** section, type ```aws-waf-logs-workshop-26```.
{{% notice note %}} 
Prefix the Kinesis Data Firehose with ```aws-waf-logs-workshop-```. This is required by the WAF service.
{{% /notice %}}
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-003.png?featherlight=false&width=90pc)
4. In the **Destination settings** section.
* Click **Browse**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-004.png?featherlight=false&width=90pc)
* Select **aws-waf-logs-001**(S3 bucket we created) is the storage.
* Click **Choose**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-005.png?featherlight=false&width=90pc)
5. Drag the screen down, Click **Create delivery stream**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-006.png?featherlight=false&width=90pc)
6. Go to [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start).
* Click **Web ACLs**.
* Select **Global**.
* Click **waf-workshop-juice-shop**.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-007.png?featherlight=false&width=90pc)
7. In the information of your **Web ACL** page.
* Select **Logging and metrics** tab.
* Click **Enable**.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-008.png?featherlight=false&width=90pc)
8. In the **Logging destination** section.
* Click **Kinesis Data Firehose stream**.
* In the **Amazon Kinesis Data Firehose delivery stream** section, Select **aws-waf-logs-workshop-26**.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-009.png?featherlight=false&width=90pc)
9. In the **Redacted fields** section.
* Select **Single header**.
* In the **Redacted headers** section, Click **Add header**
* Add the header value: ```Cookie```.
* Click **Save**
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-010.png?featherlight=false&width=90pc)
10. Run command
```
curl "<Your Juice Shop URL>?username=admin"
curl "<Your Juice Shop URL>?milkshake=banana&favourite-topping=sauce"
curl -H "x-milkshake: chocolate" "<Your Juice Shop URL>"
```
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-011.png?width=60pc)
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-012.png?width=60pc)
11. Download the logged file in S3 bucket.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-013.png?featherlight=false&width=90pc)
12. Search for the Cookie header in the logged file.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-014.png?width=50pc)
#### Conclusion
WAF allows you to capture request logs and store them in any Kinesis Data Firehose destination. The logs provide information of the request. The logs also provide the action and rule involved for a request. This information can be invaluable when running a WAF. Use field redaction to avoid logging sensitive information.