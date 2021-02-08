+++
title = "Cấu hình Log"
date = 2020
weight = 1
chapter = false
pre = "<b>6.1. </b>"
+++

**Nội dung:**
- [Thử thách](#thử-thách)
- [Cấu hình: Logging](#cấu-hình-logging)
- [Kết luận](#kết-luận)

#### Thử thách
Cửa hàng Nước ép của bạn đang phát triển nhanh chóng. Tuyệt vời! Bây giờ, bạn đang có một tập các rule, bạn dần đần cảm thấy khó nhận ra rule nào chịu trách nhiệm ngăn chặn các request. Như vậy, việc ghi nhận log trở nên hữu ích vào lúc này. Để làm việc này, bạn cần phải bật tính năng ghi nhật ký cho Web ACL của bạn vào S3 bucket.
Log của bạn chứa những header, named cookie nhạy cảm. Bạn không muốn nội dung này được ghi nhận lại. Vì vậy bạn cần phải cấu hình loại bỏ các header trong log.

{{% notice tip %}}
Tìm hiểu thêm tại tài liệu [Logging Web ACL Traffic Information](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html).
{{% /notice %}}

1. Tạo một **S3 bucket**. Đây sẽ là nơi lưu trữ đích của **Kinesis Data Firehose**.
   - Đặt ```aws-waf-logs-workshop-``` ở phía trước tên để bạn có thể tìm thấy dễ hơn sau này. (VD: aws-waf-logs-workshop-20210124)

![Logging](../../../images/6/1.png?width=90pc)

{{% notice info %}}
Khi sử dụng Kinesis Data Firehose để ghi nhận các WAF request, Firehose name cần phải có tên ```aws-waf-logs-``` ở phía trước (VD: aws-waf-logs-workshop-ds-20210125).
{{% /notice %}}

2. Tạo một Kinesis Data Firehose delivery stream. Bạn cần chắc rằng sẽ tạo trong region ```us-east-1```. [Đây là yêu cầu bắt buộc để ghi nhận log cho CloudFront](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html). 
   - Tên của Kinesis Data Firehose sẽ bắt đầu với ```aws-waf-logs-workshop-```. Đây là yêu cầu bắt buộc của dịch vụ WAF.
   - Sử dụng S3 bucket đã tạo trước đó là nơi lưu trữ đích.
3. Bật tính năng ghi nhận log cho WAF của bạn.
4. Điều chỉnh lại cookie, đặt tên là ```Cookie``` (Juice Shop Cookie) trong log của bạn.
5. Tạo ra một số các lưu lượng sử dụng các request sau. Một vài request sẽ bị chặn bởi những rule mà bạn đã tạo trước đó.

```bash
curl "$JUICESHOP_URL?username=admin"
curl "${JUICESHOP_URL}?milkshake=banana&favourite-topping=sauce"
curl -H "x-milkshake: chocolate" "${JUICESHOP_URL}"
```

6. Tải tập tin log từ S3 bucket đã được thiết lập là nơi lưu trữ cho Kinesis Data Firehose.
7. Kiểm tra tập tin log và tìm giá trị ```Cookie```

{{% notice tip %}}
Xem các vùng đã được điều chỉnh với cấu hình log sử dụng lệnh CLI [*get-logging-configuration*](https://docs.aws.amazon.com/cli/latest/reference/wafv2/get-logging-configuration.html).
{{% /notice %}}

#### Cấu hình: Logging

{{%expand "Chọn để xem các bước thực hiện" %}}
1. Tạo một Kinesis Data Firehose ở ```us-east-1```. Chắc chắn tên bắt đầu với ```aws-waf-logs-```. Ví dụ: ```aws-waf-logs-workshop```. Chọn S3 là nơi lưu trữ đích.

![Logging](../../../images/6/2.png?width=90pc)

![Logging](../../../images/6/3.png?width=90pc)

![Logging](../../../images/6/4.png?width=90pc)

![Logging](../../../images/6/5.png?width=90pc)

![Logging](../../../images/6/6.png?width=90pc)

![Logging](../../../images/6/7.png?width=90pc)

![Logging](../../../images/6/8.png?width=90pc)

2. Chọn **Web ACL** của bạn từ WAF Console
3. Chọn tab **Logging and Metrics**
4. Chọn **Enable logging**

![Logging](../../../images/6/9.png?width=90pc)

5. Chọn **Kinesis Date Firehose** mà bạn muốn sử dụng.
6. Ở trong **Redacted Fields**, chọn **Header**. Thêm vào giá trị header ```Cookie```

![Logging](../../../images/6/10.png?width=90pc)

![Logging](../../../images/6/11.png?width=90pc)

7. Chạy lệnh ```curl``` để tạo ra các luồng dữ liệu

![Logging](../../../images/6/12.png?width=90pc)

8. Tải tập tin log ghi nhận vào S3.

![Logging](../../../images/6/13.png?width=90pc)

9. Tìm từ khóa ```Cookie``` trong tập tin.

![Logging](../../../images/6/14.png?width=90pc)
{{% /expand%}}

#### Kết luận
WAF cho phép bạn ghi nhận các nhật kí request và lưu trữ trong nơi lưu trữ đích của Kinesis Data Firehose. Nhật ký cung cấp thông tin về request. Nhật ký đồng thời cung cấp hành động và rule đã tác động lên request. Thông tin này có thể không giá trị khi bạn chạy WAF. Sử dụng tính năng điều chỉnh các vùng nội dung để loại bỏ các thông tin nhạy cảm.
