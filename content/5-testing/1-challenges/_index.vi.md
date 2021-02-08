+++
title = "Thử thách"
date = 2020
weight = 1
chapter = false
pre = "<b>5.1. </b>"
+++

**Contents:**
- [Thử thách](#thử-thách)
- [Configuration: Update your current Rule](#configuration-update-your-current-rule)
- [Test case](#test-case)

#### Thử thách
Bạn đã định nghĩa được một rule mới cho WAF của bạn. Trước khi triển khai chúng, bạn cần phải kiểm thử trước. Việc này nhằm giảm nguy cơ bạn vô tình chặn các request hợp lệ.

Rule bên dưới sẽ chặn các request với thông số truy xuất vào username.

[Đọc thêm về kiểm thử Web ACLs](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html)

1. Cập nhật hành động của Rule thành Count

{{%expand "Chọn để xem hướng dẫn thực hiện" %}}
```json
{
  "Name": "count-von-count",
  "Priority": 0,
  "Action": {
    "Block": {}
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
{{% /expand%}}

2. Triển khai rule của bạn thông qua CLI hoặc Console.

#### Configuration: Update your current Rule

{{%expand "Chọn để xem hướng dẫn thực hiện" %}}
1. Cập nhật hành động của Rule thành Count

```json
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

![Test Rule](../../../images/5/1.png?width=90pc)

2. Tạo một rule mới trong console, Đổi sang sử dụng trình biên soạn JSON và nhập rule.
3. Gửi một số request chứa thông số ```username``` đến Ứng dụng web của bạn.
4. Truy xuất đến trang **CloudWatch Metrics**. Chọn **AWS/WAFv2**, sau đó chọn **Region** > **Rule** > **WebACL** để xem thông tin.

![Test Rule](../../../images/5/3.png?width=90pc)

{{% /expand%}}

#### Test case

Thực thi câu lệnh sau trong giao diện dòng lệnh.

```bash
curl "$JUICESHOP_URL?username=admin"
```

![Test Rule](../../../images/5/2.png?width=90pc)

Request này sẽ không bị chặn nhưng sẽ được ghi nhận bộ đếm. Hãy kiểm tra [CloudWatch metrics](https://console.aws.amazon.com/cloudwatch/home?#metricsV2:graph=~()) xem rule của bạn có hoạt động không.

![Test Rule](../../../images/5/4.png?width=90pc)

Trước khi triển khai một rule mới vòa Web ACL cua bạn, hãy kiểm thử nó. Kiểm thử mộtrule mới với hành cộng Count, giám sát nó thông qua CloudWatch metrics.
