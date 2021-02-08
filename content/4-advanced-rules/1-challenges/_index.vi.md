+++
title = "Thử thách Rule Nâng cao"
date = 2020
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++

**Contents:**
- [Thử thách](#thử-thách)
- [Cập nhật JSON Rule](#cập-nhật-json-rule)
- [Test Case](#test-case)

#### Thử thách
Băng nhóm Sữa Lắc lại tiếp tục tấn công ứng dụng của bạn. Chúng đã thay đổi cách tấn công một lần nữa! Bạn cần phải tạo ra một rule mới để chặn các request xấu xa này trong khi vẫn cho phép khách hàng thực sự gửi request.

Lần này, bạn sẽ khởi đầu với rule căn bản như sau:

{{%expand "Chọn để xem rule khởi đầu" %}}
```json
{
  "Name": "complex-rule-challenge",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-challenge"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "ByteMatchStatement": {
            "FieldToMatch": {
              "SingleHeader": {
                "Name": "x-milkshake"
              }
            },
            "PositionalConstraint": "EXACTLY",
            "SearchString": "chocolate",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        },
        {
          "ByteMatchStatement": {
            "FieldToMatch": {
              "SingleQueryArgument": {
                "Name": "milkshake"
              }
            },
            "PositionalConstraint": "EXACTLY",
            "SearchString": "banana",
            "TextTransformations": [
              {
                "Type": "NONE",
                "Priority": 0
              }
            ]
          }
        }
      ]
    }
  }
}
```
{{% /expand%}}

Hiện tại, rule này sẽ chặn tất cả các request thỏa hai điều kiện:

1. Chứa *header* ```x-milkshake: chocolate```
2. Chứa *query parameter* ```milkshake=banana```

Rule này hoạt động, nhưng kẻ tấn công đã thay đổi phương pháp. Các request xấu bây giờ bao gồm như sau:

1. Chứa *header* ```x-milkshake: chocolate``` và cả *header* ```x-favourite-topping: nuts```
2. Chứa *query parameter* ```milkshake=banana``` và cả *query parameter* ```favourite-topping=sauce```

Bạn cần cập nhật lại rule hiện tại bằng việc sử dụng *AndStatement* để mở rộng rule hiện tại với 2 statement. Cấu trúc của *AndStatement* như sau:

```json
"AndStatement": {
  "Statements": [
    # Statement sẽ viết ở đây
  ]
}
```

#### Cập nhật JSON Rule

Sau khi cập nhật lại rule, chúng ta sẽ có rule cuối cùng như sau:

{{%expand "Chọn để xem rule cập nhật" %}}
```json
{
  "Name": "complex-rule-challenge",
  "Priority": 0,
  "Action": {
    "Block": {}
  },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "complex-rule-challenge"
  },
  "Statement": {
    "OrStatement": {
      "Statements": [
        {
          "AndStatement": {
            "Statements": [
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleHeader": {
                      "Name": "x-milkshake"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "chocolate",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              },
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleHeader": {
                      "Name": "x-favourite-topping"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "nuts",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "AndStatement": {
            "Statements": [
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleQueryArgument": {
                      "Name": "milkshake"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "banana",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              },
              {
                "ByteMatchStatement": {
                  "FieldToMatch": {
                    "SingleQueryArgument": {
                      "Name": "favourite-topping"
                    }
                  },
                  "PositionalConstraint": "EXACTLY",
                  "SearchString": "sauce",
                  "TextTransformations": [
                    {
                      "Type": "NONE",
                      "Priority": 0
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

![Advanced Rule](../../../images/4/1.png?width=90pc)

![Advanced Rule](../../../images/4/2.png?width=90pc)
{{% /expand%}}

#### Test Case

Kiểm tra hoạt động của rul emoiws bằng việc chạy các câu lệnh sau:

```bash
# Set the JUICESHOP_URL if not already done
export JUICESHOP_URL=<Your Juice Shop URL>
```

```bash
# Allowed
curl -H "x-milkshake: chocolate" "${JUICESHOP_URL}"
curl  "${JUICESHOP_URL}?milkshake=banana"
```

![Advanced Rule](../../../images/4/3.png?width=90pc)

![Advanced Rule](../../../images/4/4.png?width=90pc)

```bash
# Blocked
curl -H "x-milkshake: chocolate" -H "x-favourite-topping: nuts" "${JUICESHOP_URL}"
curl  "${JUICESHOP_URL}?milkshake=banana&favourite-topping=sauce"
```

![Advanced Rule](../../../images/4/5.png?width=90pc)

![Advanced Rule](../../../images/4/6.png?width=90pc)

Các request bị chặn sẽ có phản hồi như sausau:

```bash
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
    <title>ERROR: The request could not be satisfied</title>
  </head>
  <body>
    <h1>403 ERROR</h1>
    <!-- Omitted -->
  </body>
</html>
```

Ở phần này, bạn đã hiểu được cách để định nghĩa WAF rule ở định dạng JSON. Các logic phức tạp cần phải được định nghĩa sử dụng các toán tử ```AND```, ```OR``` và ```NOT```.
