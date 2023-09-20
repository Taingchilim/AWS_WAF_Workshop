+++
title = "Deploy the sample Web App"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++

#### Deploy the Sample Web App

In this workshop, you will use the [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/). The Juice Shop is an Open Source web application that is intentionally insecure. [Pwning OWASP Juice Shop](https://pwning.owasp-juice.shop/) is a free book that explains the app and its vulnerabilities in more detail.

1. Choose a region to deploy the Sample Web App to, and follow the appropriate link from the table below.

   | Region | Launch Template |
   | ------ | --------------- |
   | US East (N. Virginia) (us-east-1) | [![Deploy to AWS](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-1.s3.us-east-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
   | US East (Ohio) (us-east-2) | [![Deploy to AWS](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-2.s3.us-east-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
   | US West (Oregon) (us-west-2) | [![Deploy to AWS](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-west-2.s3.us-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
   | EU (Ireland) (eu-west-1) | [![Deploy to AWS](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-1.s3.eu-west-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
   | EU (London) (eu-west-2) | [![Deploy to AWS](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-2.s3.eu-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |

2. Click **Next**.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-001.png?featherlight=false&width=90pc)

3. In the **Specify stack details** page, click **Next**.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-002.png?featherlight=false&width=90pc)

4. In the **Configure stack options** page, click **Next**.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-003.png?featherlight=false&width=90pc)

5. In the **Review WAFWorkshopSampleWebApp** page:

   - Drag the screen down.
   - Click **I acknowledge that AWS CloudFormation might create IAM resources with custom names**.
   - Click **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**.
   - Click **Create stack**.

   ![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-004.png?featherlight=false&width=90pc)

   {{% notice note %}}
   **Cloudformation** will take 5 minutes to deploy Juice Shop App. Wait until all stacks are shown in a **CREATE_COMPLETE** state.
   {{% /notice %}}

   ![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-005.png?featherlight=false&width=90pc)

6. Click **Output**, Click **dkievcmqb5kzc.cloudfront.net** (URL of Juice Shop web) to go to your app.

   ![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-006.png?featherlight=false&width=90pc)
   ![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-007.png?featherlight=false&width=90pc)
