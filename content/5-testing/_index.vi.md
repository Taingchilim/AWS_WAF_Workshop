+++
title = "Kiểm thử Rule mới"
date = 2020
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

Trước khi triển khai một rule mới, bạn cần phải kiểm thử để chắc chắn bạn không vô tình chặn các request hợp lệ.

Ở những phần trước, bạn đã sử dụng *Block* và *Allow* khi định nghĩa các hành động khi đánh giá request. Bên cạnh đó, bạn có một lựa chọn khác là *Count*. *Count* cho phép bạn đánh giá số lượng request trùng với điều kiện rule của bạn.

*Count* không phải là hành động ngăn chặn. Khi một request khớp rule có hành động *Count*, Web ACL sẽ tiếp tục xử lý các rule khác.

{{% notice tip %}}
Các rule hoặc nhóm rule dựng sẵn cũng có thể kiểm thử sử dụng *Count*.
{{% /notice %}}

**Nội dung**
- [Xem bộ đếm Rule](#xem-bộ-đếm-rule)

#### Xem bộ đếm Rule
Khi một rule có hành động *Count* khớp với request, sự kiện này được thêm vào CloudWatch metrics. Để xem bộ đếm của một rule, bạn truy cập vào [CloudWatch metrics console](https://console.aws.amazon.com/cloudwatch/home?#metricsV2:graph=~()). Chọn **AWS/WAFv2**, sau đó chọn **Region** > **Rule** > **WebACL** để xem metric đó.

{{% notice tip %}}
Mặc định, giá trị *Average* sẽ được hiển thị các metric WAF. Bạn có thể đổi sang giá trị *Sum* để phân tích đánh giá.
{{% /notice %}}
