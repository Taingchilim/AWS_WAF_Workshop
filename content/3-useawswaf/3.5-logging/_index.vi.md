+++
title = "Ghi nhật ký request"
date = 2020
weight = 5
chapter = false
pre = "<b>3.5. </b>"
+++
#### Tình huống
Cửa hàng Nước ép của bạn đang phát triển nhanh chóng. Tuyệt vời! Bây giờ, bạn đang có một tập các rule, bạn dần đần cảm thấy khó nhận ra rule nào chịu trách nhiệm ngăn chặn các request. Như vậy, việc ghi nhận log trở nên hữu ích vào lúc này. Để làm việc này, bạn cần phải bật tính năng ghi nhật ký cho Web ACL của bạn vào S3 bucket. Log của bạn chứa những header, named cookie nhạy cảm. Bạn không muốn nội dung này được ghi nhận lại. Vì vậy bạn cần phải cấu hình loại bỏ các header trong log.

#### Ghi nhật ký request
{{% notice info %}} 
WAF sử dụng [Amazon Kinesis Firehose](https://aws.amazon.com/kinesis/data-firehose/) để ghi nhật ký. Điều này cho phép các bản ghi được chuyển đến bất kỳ kho Kinesis Firehose nào, chẳng hạn như Amazon S3, Amazon Redshift hoặc Amazon Elastic Search. Để ghi nhật ký các yêu cầu trong Web ACL của bạn, bạn phải tạo một Kinesis Data Firehose.
{{% /notice %}}
1. Truy cập [Amazon Kinesis Console](https://us-east-1.console.aws.amazon.com/kinesis/home?region=us-east-1#/home).
* Click **Kinesis Data Firehose**.
* Click **Create delivery stream**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-001.png?width=90pc)
{{% notice note %}} 
Bạn cần chắc rằng sẽ tạo trong region **us-east-1**. [Đây là yêu cầu bắt buộc để ghi nhận log cho CloudFront](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html).
{{% /notice %}}
2. Trong phần **Choose source and destination**.
* Tại mục **Source** Chọn **Direct PUT**.
* Tại mục **Destination** Chọn **Amazon S3**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-002.png?width=90pc)
3. Tại mục **Choose source and destination** điền ```aws-waf-logs-workshop-26```.
{{% notice note %}} 
Tên của Kinesis Data Firehose sẽ bắt đầu với ```aws-waf-logs-workshop-```. Đây là yêu cầu bắt buộc của dịch vụ WAF.
{{% /notice %}}
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-003.png?width=90pc)
4. Trong phần **Destination settings**.
* Click **Browse**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-004.png?width=90pc)
* Chọn **aws-waf-logs-001**(S3 bucket đã tạo trước đó) là nơi lưu trữ đích.
* Click **Choose**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-005.png?width=90pc)
5. Kéo màn hình xuống dưới, Click **Create delivery stream**.
![Create Kinesis Data Firehose](/images/3-useawswaf/3.5-logging/logging-006.png?width=90pc)
6. Truy cập [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start).
* Click **Web ACLs**.
* Chọn **Global**.
* Click **waf-workshop-juice-shop**.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-007.png?width=90pc)
7. Ở trang thông tin **Web ACL** của bạn.
* Chọn tab **Logging and metrics**.
* Click **Enable**.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-008.png?width=90pc)
8. Trong phần **Logging destination**.
* Click **Kinesis Data Firehose stream**.
* Tại mục **Amazon Kinesis Data Firehose delivery stream** Chọn **aws-waf-logs-workshop-26**.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-009.png?width=90pc)
9. Trong phần **Redacted fields**.
* Chọn **Single header**.
* Tại mục **Redacted headers** Click **Add header**
* Thêm vào giá trị header ```Cookie```.
* Click **Save**
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-010.png?width=90pc)
10. Chạy lệnh
```
curl "<Your Juice Shop URL>?username=admin"
curl "<Your Juice Shop URL>?milkshake=banana&favourite-topping=sauce"
curl -H "x-milkshake: chocolate" "<Your Juice Shop URL>"
```
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-011.png?width=60pc)
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-012.png?width=60pc)
11. Tải tập tin log ghi nhận vào S3.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-013.png?width=90pc)
12. Tìm từ khóa Cookie trong tập tin.
![Use Kinesis](/images/3-useawswaf/3.5-logging/logging-014.png?width=50pc)
#### Kết luận
WAF cho phép bạn ghi nhận các nhật kí request và lưu trữ trong nơi lưu trữ đích của Kinesis Data Firehose. Nhật ký cung cấp thông tin về request. Nhật ký đồng thời cung cấp hành động và rule đã tác động lên request. Thông tin này có thể không giá trị khi bạn chạy WAF. Sử dụng tính năng điều chỉnh các vùng nội dung để loại bỏ các thông tin nhạy cảm.