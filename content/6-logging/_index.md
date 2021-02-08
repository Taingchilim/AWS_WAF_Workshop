+++
title = "Logging"
date = 2020
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

WAF Uses [Amazon Kinesis Firehose](https://aws.amazon.com/kinesis/data-firehose/) to ingest logs. This allows logs to be passed to any Kinesis Firehose destination, such as Amazon S3, Amazon Redshift or Amazon Elastic Search. To enable logging of requests in your Web ACL, you must first create a Kinesis Data Firehose.

**Content**
- [WAF Log Example](#waf-log-example)
- [Analysing and Visualising Logs](#analysing-and-visualising-logs)

#### WAF Log Example

Here is an example WAF log of a request. Note that you receive details on the rule that terminated the evaluation of the request. The log also contains the action taking on that request

```json
{
  "timestamp": 1576280412771,
  "formatVersion": 1,
  "webaclId": "arn:aws:wafv2:ap-southeast-2:EXAMPLE12345:regional/webacl/STMTest/1EXAMPLE-2ARN-3ARN-4ARN-123456EXAMPLE",
  "terminatingRuleId": "STMTest_SQLi_XSS",
  "terminatingRuleType": "REGULAR",
  "action": "BLOCK",
  "terminatingRuleMatchDetails": [
    {
      "conditionType": "SQL_INJECTION",
      "location": "HEADER",
      "matchedData": ["10", "AND", "1"]
    }
  ],
  "httpSourceName": "-",
  "httpSourceId": "-",
  "ruleGroupList": [],
  "rateBasedRuleList": [],
  "nonTerminatingMatchingRules": [],
  "httpRequest": {
    "clientIp": "1.1.1.1",
    "country": "AU",
    "headers": [
      {
        "name": "Host",
        "value": "localhost:1989"
      },
      {
        "name": "User-Agent",
        "value": "curl/7.61.1"
      },
      {
        "name": "Accept",
        "value": "*/*"
      },
      {
        "name": "x-stm-test",
        "value": "10 AND 1=1"
      }
    ],
    "uri": "/foo",
    "args": "",
    "httpVersion": "HTTP/1.1",
    "httpMethod": "GET",
    "requestId": "rid"
  }
}

```

#### Analysing and Visualising Logs
Log analysis is key to ensure the effectiveness of WAF Rules, in addition to troubleshooting of individual issues. Logs exported to [Amazon Elastic Search Service](https://aws.amazon.com/elasticsearch-service/) can be queried. [Kibana](https://aws.amazon.com/elasticsearch-service/the-elk-stack/kibana/) can be used alongside Amazon Elastic Search to visualise the WAF logs [S3 Select](https://docs.aws.amazon.com/AmazonS3/latest/API/API_SelectObjectContent.html) can be used to perform SQL queries against individual WAF Log files in S3. Queries can be performed in the console, or using the AWS CLI or SDKs.

For example, the following S3 Select query counts how many requests in the log file were blocked.

```sql
SELECT *
FROM S3Object s
WHERE  s."action" = 'BLOCK'

/*
Result:
{
    "count": 18
}
*/
```