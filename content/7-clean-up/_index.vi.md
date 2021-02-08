+++
title = "Thu hồi tài nguyên"
date = 2020
weight = 7
chapter = false
pre = "<b>7. </b>"
+++

Trước khi bạn kết thúc bài thực hành này, hãy đảm bảo xóa các tài nguyên bạn không cần nữa.

Dưới đây là danh sách các tài nguyên bạn đã tạo trong bài thực hành này.

- [Ứng dụng Web Mẫu](#ứng-dụng-web-mẫu)
- [WAF web ACL](#waf-web-acl)
- [Kinesis Data Firehose](#kinesis-data-firehose)
- [S3 Bucket](#s3-bucket)

Dưới đây là hướng dẫn về cách xóa các tài nguyên này.

#### Ứng dụng Web Mẫu
Ứng dụng Web Mẫu được định nghĩa trong CloudFormation stack, đặt tên là WAFWorkshopSampleWebApp.

Làm theo các bước trong tài liệu [Xóa một CloudFormation Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html).

{{% notice tip %}}
**Ứng dụng Web Mẫu** là một nested stack. Bằng cách xóa stack chính, các nnested stack được chứa bên trong cũng sẽ bị xóa.
{{% /notice %}}

#### WAF web ACL
Làm theo các bước trong tài liệu [Xóa một Web ACL](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-deleting.html).

#### Kinesis Data Firehose

1. Truy cập vào trang [Kinesis console](https://console.aws.amazon.com/firehose/home/dashboard/list)
2. Chọn tab **Data Firehose**. Nếu bạn không thể thấy tài nguyên, hãy kiểm tra region của bạn. Tài nguyên này phải ở ```us-east-1```. Điều này có thể khác với khu vực được sử dụng để triển khai Ứng dụng web
3. Xóa Data Firehose bạn đã tạo trước đó. Nó sẽ có tiền tố là ```aws-waf-logs-workshop-```

#### S3 Bucket

1. Truy cập đến S3 Console.
2. Chọn S3 bucket được sử dụng làm đích Kinesis Data Firehose. Nó sẽ có tiền tố ```aws-waf-logs-workshop-``` Nếu bạn không thể thấy tài nguyên, hãy kiểm tra region của bạn. Tài nguyên này phải ở ```us-east-1```. Điều này có thể khác với khu vực được sử dụng để triển khai Ứng dụng web
3. Xóa các tập tin trong bucket.
4. Xóa bucket.
