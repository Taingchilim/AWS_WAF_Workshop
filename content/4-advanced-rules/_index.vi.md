+++
title = "Rule Tùy chỉnh (Nâng cao)"
date = 2020
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

You’ve created a simple WAF rule that evaluates one part of a request. How could you create a rule to evaulate multiple parts of a request?

**Contents:**
- [Định nghĩa Rule bằng JSON](#định-nghĩa-rule-bằng-json)
- [Logic luận lý trong Rule](#logic-luận-lý-trong-rule)
- [Ví dụ](#ví-dụ)

#### Định nghĩa Rule bằng JSON
Tất cả WAF Rule được định nghĩa là một JSON Object. Đối với các rule phức tạp, bạn sẽ dễ dàng xử lý hơn nếu làm việc trực tiếp ở định dạng JSON thay vì sử dụng Rule Editor trên console. Bạn có thể lấy các thông tin của rule hiện tại được định nghĩa bằng JSON thông qua sử dụng bằng API, CLI hoặc Console sử dụng lệnh [get-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/get-rule-group.html). Chỉnh sửa chúng sử dụng trình biên soạn JSON mà bạn muốn và tải chúng lên với lệnh [update-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/update-rule-group.html) sử dụng API, CLI hoặc Console.

Định nghĩa rule sử dụng JSON cho phép bạn áp dụng việc quản lý phiên bản nhằm dễ dàng xem xét cách thức, thời điểm và lý do một tập các rule phức tạp được thay đổi.

{{% notice tip %}}
Cấu trúc để định nghĩa rule trong JSON được cung cấp trong tài liệu [update-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/update-rule-group.html)
{{% /notice %}}

Nếu bạn chưa chắc chắn về cấu trúc, bạn có thể sử dụng Console visual editor để tạo một rule đơn giản sau đó chuyển sang JSON editor để xem dưới dạng JSON.

#### Logic luận lý trong Rule

Các toán tử ```AND```, ```OR``` và ```NOT``` có thể được sử dụng cho các trường hợp phức tạp. Việc này sẽ hữu ích khi phân tích nhiều phần của một request. Ví dụ như bạn có thể chỉ cho phép một request nếu như chuỗi truy xuất hoặc header chứa một khóa/giá trị nhất định.

{{% notice tip %}}
Các rule lồng nhau có thể được tạo ra sử dụng visual editor. Tuy nhiên chúng chỉ giới hạn ở 1 lớp lồng nhau.  
Để tạo các rule lồng nhau tùy biến trong console, bạn bắt buộc phải sử dụng JSON.  
Sử dụng lựa chọn Validate để kiểm tra tính chính xác của rule đó.
{{% /notice %}}

Bên dưới là một ví dụ của rule được tạo trong console. Rule này sẽ chặn các request với chuỗi truy xuất với độ dài lớn hơn hoặc bằng 0.

{{% notice info %}}
Rule này sẽ chặn tất cả các request chứa chuỗi truy xuất dữ liệu.
{{% /notice %}}

Đây là rule được tạo dưới định dạng JSON.
- Các hành động được xác định bởi hành động của WAF nếu request chính xác với định nghĩa của rule.
- VisibilityConfig được sử dụng để cấu hình các mẫu request và CloudWatch metrics
- Statement định nghĩa các điều kiện để đánh giá rule.

```json
{
  "Name": "example-rule-01",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "example-rule-01"
  },
  "Statement": {
    "SizeConstraintStatement": {
      "FieldToMatch": {
        "QueryString": {}
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

#### Ví dụ

Đồng nghiệp của bạn đang cần tư vấn của bạn, khi họ đang gặp phải một tấn công. Họ cần chặn các request tấn công đồng thời không chặn các request đến từ các khách hàng thực sự của họ. Các request tấn công chứa nội dung có kích thước lớn hơn 100KB, không có header x-upload-photo: true.

Bạn nhanh chóng nhận ra không thể định nghĩa rule này sử dụng rule editor ở giao diện console. Bạn cần phải sử dụng đến JSON.

Bạn bắt đầu với một rule trắng:

```json
{
  "Name": "example-rule-02",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "example-rule-02"
  },
  "Statement": {
    // Chúng ta sẽ định nghĩa các statement ở đây
  }
}
```

Có hai trường hợp cần phải cân nhắc như sau:
1. Nếu như nội dung request lớn hơn 100KB, chặn request đó.
2. Nếu như header của request không chứa cặp khóa/giá trị ```x-upload-body: true```, chặn request đó.

Chúng ta sẽ cần phải sử dụng đến [OrStatement](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-or.html) và [NotStatement](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-not.html) để mô tả phần logic này.

```json
{
// fields omitted for brevity
"Statement": {
  "OrStatement": {
    "Statements": [
      {
        // Inspect Body Size here
      },
      {
        "NotStatement": {
           // Inspect Header here
        }
      }
    ]
  }
}
```

Để phân tích kích thước nội dung, chúng ta sẽ sử dụng SizeConstraintStatement để xem xét request.

```json
"SizeConstraintStatement": {
  "FieldToMatch": {
    "Body": {}
  },
  "ComparisonOperator": "GT",
  "Size": "100",
  "TextTransformations": [
    {
      "Type": "NONE",
      "Priority": 0
    }
  ]
}

```

Để phân tích Header của request, chúng ta sẽ sử dụng ByteMatchStatement.

```json
"ByteMatchStatement": {
  "FieldToMatch": {
    "SingleHeader": {
      "Name": "x-upload-image"
    }
  },
  "PositionalConstraint": "EXACTLY",
  "SearchString": "true",
  "TextTransformations": [
    {
      "Type": "NONE",
      "Priority": 0
    }
  ]
}
```

Và cuối cùng chúng ta sẽ được rule như sau:

```json
{
  "Name": "complex-rule-example",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-example"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "SizeConstraintStatement": {
            "FieldToMatch": {
              "Body": {}
            },
            "ComparisonOperator": "GT",
            "Size": "100",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        },
        {
          "NotStatement": {
            "Statement": {
              "ByteMatchStatement": {
                "FieldToMatch": {
                  "SingleHeader": {
                    "Name": "x-upload-body"
                  }
                },
                "PositionalConstraint": "EXACTLY",
                "SearchString": "true",
                "TextTransformations": [
                  {
                    "Type": "NONE",
                    "Priority": 0
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}
```
