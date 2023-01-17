+++
title = "Kiểm thử Rule mới"
date = 2020
weight = 4
chapter = false
pre = "<b>3.4. </b>"
+++

Trước khi triển khai một rule mới, bạn cần phải kiểm thử để chắc chắn bạn không vô tình chặn các request hợp lệ.

Ở những phần trước, bạn đã sử dụng Block và Allow khi định nghĩa các hành động khi đánh giá request. Bên cạnh đó, bạn có một lựa chọn khác là Count. Count cho phép bạn đánh giá số lượng request trùng với điều kiện rule của bạn.

Count không phải là hành động ngăn chặn. Khi một request khớp rule có hành động Count, Web ACL sẽ tiếp tục xử lý các rule khác.

#### Tình huống
Bạn đã định nghĩa được một rule mới cho WAF của bạn. Trước khi triển khai chúng, bạn cần phải kiểm thử trước. Việc này nhằm giảm nguy cơ bạn vô tình chặn các request hợp lệ.

Rule bên dưới sẽ chặn các request với thông số truy xuất vào username.
#### Kiểm thử Rule mới
1. Tạo một rule mới tương tự như [tạo Custom Rule nâng cao trong phần 3.3](../3.3-createadvancecustomrule/).
* Tại mục **JSON** điền 
```
{
  "Name": "count-von-count",
  "Priority": 0,
  "Action": {
    "Count": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "count-von-count"
  },
  "Statement": {
    "SizeConstraintStatement": {
      "FieldToMatch": {
        "SingleQueryArgument": {
          "Name": "username"
        }
      },
      "ComparisonOperator": "GT",
      "Size": "0",
      "TextTransformations": [
        {
          "Type": "NONE",
          "Priority": 0
        }
      ]
    }
  }
}
```
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-001.png?featherlight=false&width=90pc)
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-002.png?featherlight=false&width=90pc)
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-003.png?featherlight=false&width=90pc)
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/createadvancecustomrule-004.png?featherlight=false&width=90pc)

2. Chạy lệnh.
```
curl "<Your Juice Shop URL>?username=admin"
```
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-001.png?width=60pc)
3. Truy xuất đến trang [CloudWatch Metrics](https://console.aws.amazon.com/cloudwatch/home?#metricsV2:graph=~()).
* Click **WAFv2**
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-002.png?featherlight=false&width=90pc)
* Click **Rule, WebACL**
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-003.png?featherlight=false&width=90pc)
* Chọn **count-von-count**, Chúng ta sẽ thấy 1 request trong phần **Untitled graph**.
![Testing new rule](/images/3-useawswaf/3.4-testingnewrule/testingnewrule-004.png?featherlight=false&width=90pc)

Trước khi triển khai một rule mới vòa Web ACL cua bạn, hãy kiểm thử nó. Kiểm thử một rule mới với hành cộng Count, giám sát nó thông qua CloudWatch metrics.
