+++
title = "Các bước chuẩn bị"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

**Nội dung:**
- [Cài đặt](#cài-đặt)
- [Ứng dụng Juice Shop](#ứng-dụng-juice-shop)
- [Triển khai một Ứng dụng mẫu](#triển-khai-một-ứng-dụng-mẫu)

#### Cài đặt
**Mac và Linux OS**

- Việc cài đặt AWS CLI là hữu dụng nhưng không cần thiết.
  - AWS CLI sẽ giúp bạn triển khai các rule tùy chỉnh nhanh hơn trong quá trình thực hiện.
  - Hãy đảm bảo bạn đang cài đặt AWS CLI phiên bản mới nhất.

**Windows**

Ở bài thực hành này bạn sẽ dùng ```curl``` để gửi các HTTP request dùng để test các WAF rule. ```curl``` có ở trong Windows Subsystem For Linux.

{{% notice tip %}}
Nếu như bạn chưa chắc chắn, bạn có thể sử dụng [AWS Cloud9 dev environment](https://console.aws.amazon.com/cloud9/home/product) để hoàn thành bài thực hành này. Cloud9 là môi trường có đầy đủ các công cụ yêu cầu. Với thiết lập mặc định khi khởi tạo, Cloud9 phù hợp với bài thực hành này.
{{% /notice %}}

#### Ứng dụng Juice Shop

AWS WAF được dùng để triển khai cùng với các Dịch vụ AWS khác như CloudFront distribution, Application Load Balancer hay API Gateway để phục vụ cho ứng dụng web của bạn.

Để có thể kiểm thử WAF, bạn cần thiết phải có một ứng dụng.

Ở trong bài thực hành này, bạn sẽ sử dụng [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/). Đây là một ứng dụng web mã nguồn mở không bảo mật.

Bạn có thể tham khảo thêm tài liệu [Pwning OWASP Juice Shop](https://pwning.owasp-juice.shop/) để biết thêm về ứng dụng và những lỗ hổng của nó chi tiết hơn.

#### Triển khai một Ứng dụng mẫu
Chọn Region để triển khai Ứng dụng Web theo bảng bên dưới.

Thời gian để CloudFormations stack triển khai sẽ mất tầm 5 phút.

| Region                            | Launch Template |
|-----------------------------------|-----------------|
| US East (N. Virginia) (us-east-1) | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-1.s3.us-east-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US East (Ohio) (us-east-2)        | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-east-2.s3.us-east-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| US West(Oregon) (us-west-2)       | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-us-west-2.s3.us-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (Ireland) (eu-west-1)          | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-1.s3.eu-west-1.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |
| EU (London) (eu-west-2)           | [![CloudEndure](../../../images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/new?stackName=WAFWorkshopSampleWebApp&templateURL=https%3a%2f%2faws-waf-workshop-v2-eu-west-2.s3.eu-west-2.amazonaws.com%2faws-waf-v2-workshop%2flatest%2fmain.template) |

{{%attachments title="Launch Templates" pattern=".*(template)"/%}}

**Các bước thực hiện:**

1. Truy cập vào các **CloudFormation stack** ở phía trên (tùy theo Region) để tiến hành triển khai.

![CloudFormation](../../../images/1/1.png?width=90pc)

2. Nếu muốn, bạn có thể thiết lập **stack** của bạn với một tên riêng (Tối đa 64 ký tự)

![CloudFormation](../../../images/1/2.png?width=90pc)

3. Chọn **Next** ở cuối trang và giữ các thiết lập mặc định.

![CloudFormation](../../../images/1/3.png?width=90pc)

4. Ở trang cuối cùng, hãy nhớ chọn vào lựa chọn **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** và **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**
5. Chọn **Create stack** để triển khai stack.

![CloudFormation](../../../images/1/4.png?width=90pc)

![CloudFormation](../../../images/1/5.png?width=90pc)

{{% notice note %}}
Cloudformation sẽ triển khai ứng dụng Juice Shop lên. Hãy đợi cho đến khi tất cả các stack ở trạng thái ```CREATE_COMPLETE```.
{{% /notice %}}

![CloudFormation](../../../images/1/6.png?width=90pc)

5. Tìm giá trị của **JuiceShopUrl** trong kết quả xuất ra sau khi chạy CloudFormation. Đó là địa chỉ trang web Juice Shop.

![CloudFormation](../../../images/1/7.png?width=90pc)

6. Cấu hình biến môi trường ```JUICESHOP_URL``` ở giao diện cửa số lệnh của bạn. Bạn sẽ sử dụng URL này để chạy các yêu cầu đến WAF của bạn.

```bash
export JUICESHOP_URL=<Your JuiceShopUrl CloudFormation Output>
```

Bạn có thể truy cập thử trang ứng dụng của bạn.

![CloudFormation](../../../images/1/8.png?width=90pc)

{{% notice tip %}}
Bạn có thể tham khảo thêm về WAF với tài liệu [AWS WAF documentation](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html).
{{% /notice %}}