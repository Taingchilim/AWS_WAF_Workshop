+++
title = "Triển khai một Ứng dụng mẫu"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++

#### Triển khai một Ứng dụng mẫu

Trong bài thực hành này, bạn sẽ sử dụng [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/). Đây là một ứng dụng web mã nguồn mở không bảo mật.

Bạn có thể tham khảo thêm tài liệu [Pwning OWASP Juice Shop](https://pwning.owasp-juice.shop/) để biết thêm về ứng dụng và những lỗ hổng của nó chi tiết hơn.

1. Truy cập vào các **CloudFormation stack** ở phía dưới (tùy theo Region) để tiến hành triển khai.

| Region &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Launch Template                                                                                                                                                                                                                                                                                             |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| US East (N. Virginia) (us-east-1)                                                                                                                                                                                                                               | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-1.s3.us-east-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US East (Ohio) (us-east-2)                                                                                                                                                                                                                                      | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-2.s3.us-east-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US West(Oregon) (us-west-2)                                                                                                                                                                                                                                     | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-west-2.s3.us-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (Ireland) (eu-west-1)                                                                                                                                                                                                                                        | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-1.s3.eu-west-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (London) (eu-west-2)                                                                                                                                                                                                                                         | [![Deploy to aws](/images/deploytoaws.png?width=10pc)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-2.s3.eu-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |

2. Click **Next**.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-001.png?featherlight=false&width=90pc)

3. Tại trang **Specify stack details**, click **Next**. 

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-002.png?featherlight=false&width=90pc)

4. Tại trang **Configure stack options**, click **Next**.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-003.png?featherlight=false&width=90pc)

5. Tại trang **Review WAFWorkshopSampleWebApp**:
   - Kéo màn hình xuống dưới.
   - Click **I acknowledge that AWS CloudFormation might create IAM resources with custom names**.
   - Click **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**.
   - Click **Create stack**.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-004.png?featherlight=false&width=90pc)

**Lưu ý:** CloudFormation sẽ mất khoảng 5 phút để triển khai ứng dụng Juice Shop. Hãy đợi cho đến khi tất cả các stack ở trạng thái **CREATE_COMPLETE**.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-005.png?featherlight=false&width=90pc)

6. Click **Output**, click **dkievcmqb5kzc.cloudfront.net** (địa chỉ trang web Juice Shop) để truy cập thử trang ứng dụng của bạn.

![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-006.png?featherlight=false&width=90pc)
![Create the sample web app](/images/2-prepare/2.2-createthesamplewebapp/createthesamplewebapp-007.png?featherlight=false&width=90pc)
