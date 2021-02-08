+++
title = "Prerequiste"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

**Contents:**
- [Setting up](#setting-up)
- [The Juice Shop](#the-juice-shop)
- [Deploy the sample Web App](#deploy-the-sample-web-app)

#### Setting up

{{% notice info %}}
This workshop requires an AWS Account.
{{% /notice %}}

**Mac and Linux OS**

- The AWS CLI may be useful, but is not mandatory
  - The AWS CLI will make it quick to deploy custom rules later in the workshop
  - Make sure the AWS CLI is updated to the latest version

**Windows**

- This workshop uses ```curl``` to create and send HTTP requests. These are to test the WAF rules. ```curl``` is present on Windows Subsystem For Linux.

{{% notice tip %}}
If you are unsure, you are recommended to use a [AWS Cloud9 dev environment](https://console.aws.amazon.com/cloud9/home/product) to complete this workshop. The Cloud9 environment contains all the tools required. The default settings when creating a Cloud9 environment are suitable for this workshop
{{% /notice %}}

#### The Juice Shop

AWS WAF is used by attaching it to another AWS Resource: either a CloudFront distribution, Application Load Balancer, or API Gateway that is associated with your web application.

In order to test your WAF, you will need an application!

In this workshop you will use the [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/). The Juice Shop is an Open Source web application that is intentionally insecure.

[Pwning OWASP Juice Shop](https://pwning.owasp-juice.shop/) is a free book that explains the app and its vulnerabilities in more detail.

#### Deploy the sample Web App
Choose a region to deploy the Sample Web App to, and follow the appropriate link from the table below.

This CloudFormations tack will take approximately 5 minutes to complete.

|               Region              | Launch Template |
|:---------------------------------:|:---------------:|
| US East (N. Virginia) (us-east-1) | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-1.s3.us-east-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US East (Ohio) (us-east-2)        | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-2.s3.us-east-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US West(Oregon) (us-west-2)       | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-west-2.s3.us-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (Ireland) (eu-west-1)          | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-1.s3.eu-west-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (London) (eu-west-2)           | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-2.s3.eu-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |

{{%attachments title="Launch Templates" pattern=".*(template)"/%}}

**Step by step instructions:**

1. If desired, provide your stack with a unique name. Be careful not to exceed the 64-character stack name limit

![CloudFormation](../../../images/1/1.png?width=90pc)

2. Click the **Next** button at the bottom of the remaining pages, using the default values.

![CloudFormation](../../../images/1/2.png?width=90pc)

3. On the final page, ensure the tickboxes allowing AWS CloudFormation to create IAM resources with custom names are ticked.

![CloudFormation](../../../images/1/3.png?width=90pc)

4. Click the orange “Create stack” button at the bottom-right of the page to deploy the stack into your account.

![CloudFormation](../../../images/1/4.png?width=90pc)

![CloudFormation](../../../images/1/5.png?width=90pc)

{{% notice note %}}
CloudFormation will now deploy the Juice Shop application into your account. Wait until all stacks are shown in a ```CREATE_COMPLETE``` state.
{{% /notice %}}

![CloudFormation](../../../images/1/6.png?width=90pc)

5. Find the JuiceShopUrl value in the CloudFormation template output. This is the address of your Juice Shop site.

![CloudFormation](../../../images/1/7.png?width=90pc)

6. Set the ```JUICESHOP_URL``` variable in your terminal. You will use this variable for running test requests against your WAF.

```bash
export JUICESHOP_URL=<Your JuiceShopUrl CloudFormation Output>
```

You can try to browse your web application.

![CloudFormation](../../../images/1/8.png?width=90pc)

{{% notice tip %}}
Use the [AWS WAF documentation](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html) to help!
{{% /notice %}}