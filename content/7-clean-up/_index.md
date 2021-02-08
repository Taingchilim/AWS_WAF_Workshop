+++
title = "Cleaning Up"
date = 2020
weight = 7
chapter = false
pre = "<b>7. </b>"
+++

Before you finish this workshop, please make sure to delete the resources you no longer need.

Here are the list of resources you will have created in this workshop.

- [Sample Web Application](#sample-web-application)
- [WAF web ACL](#waf-web-acl)
- [Kinesis Data Firehose](#kinesis-data-firehose)
- [S3 Bucket](#s3-bucket)

Below are instructions on how to delete these resources.

#### Sample Web Application
The Sample Web Application is contained is defined as a CloudFormation stack, titled WAFWorkshopSampleWebApp.

Follow the steps in [Deleting a CloudFormation Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html).

{{% notice tip %}}
The *WAFWorkshopSampleWebApp* is a nested stack. By deleting the top level stack, the nested stacks contained will also be removed.
{{% /notice %}}

#### WAF web ACL
Follow the steps in [Deleting a Web ACL](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-deleting.html).

#### Kinesis Data Firehose

1. Navigate to the [Kinesis console](https://console.aws.amazon.com/firehose/home/dashboard/list)
2. Select the *Data Firehose* tab. If you cannot see the resource, double check your region. This resource should be in ```us-east-1```. This may differ from the region used for the Web App deployment
3. Delete the Data Firehose you created earlier. It will have the prefix ```aws-waf-logs-workshop-```

#### S3 Bucket

1. Navigate to the S3 Console.
2. Select the bucket used as the Kinesis Data Firehose destination. It will have the prefix ```aws-waf-logs-workshop-``` If you cannot see the resource, double check your region. This resource should be in ```us-east-1```. This may differ from the region used for the Web App deployment
3. Delete the contents of the bucket.
4. Delete the bucket.
