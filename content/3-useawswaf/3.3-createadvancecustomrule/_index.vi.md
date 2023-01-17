+++
title = "Custom Rule Nâng cao"
date = 2020
weight = 3
chapter = false
pre = "<b>3.3. </b>"
+++

#### Tình huống
Băng nhóm Sữa Lắc lại tiếp tục tấn công ứng dụng của bạn. Chúng đã thay đổi cách tấn công một lần nữa! Bạn cần phải cập nhật rule để chặn các request xấu xa này trong khi vẫn cho phép khách hàng thực sự gửi request.

#### Tạo Custom Rule Nâng cao
{{% notice info %}} 
Tất cả WAF Rule được định nghĩa là một JSON Object. Đối với các rule phức tạp, bạn sẽ dễ dàng xử lý hơn nếu làm việc trực tiếp ở định dạng JSON thay vì sử dụng Rule Editor trên console. Bạn có thể lấy các thông tin của rule hiện tại được định nghĩa bằng JSON thông qua sử dụng bằng API, CLI hoặc Console sử dụng lệnh [get-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/get-rule-group.html). Chỉnh sửa chúng sử dụng trình biên soạn JSON mà bạn muốn và tải chúng lên với lệnh [update-rule-group](https://docs.aws.amazon.com/cli/latest/reference/wafv2/update-rule-group.html) sử dụng API, CLI hoặc Console.\
Định nghĩa rule sử dụng JSON cho phép bạn áp dụng việc quản lý phiên bản nhằm dễ dàng xem xét cách thức, thời điểm và lý do một tập các rule phức tạp được thay đổi.
{{% /notice %}}

Lần này, bạn sẽ khởi đầu với rule căn bản như sau:\
Rule này sẽ chặn tất cả các request thỏa hai điều kiện:
* Chứa header ```x-milkshake: chocolate```
* Chứa query parameter ```milkshake=banana```

1. Ở trang thông tin **Web ACL** của bạn.
* Click **Rules**.
* Click **Add Rules**.
* Click **Add my own rules and rule groups**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-001.png?featherlight=false&width=90pc)
2. Trong phần **Rule builder**.
* Click **Rule JSON editor**.
* Tại mục **JSON** điền 
```
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
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-002.png?featherlight=false&width=90pc)
3. Kéo màn hình xuống dưới, Click **Add rule**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-003.png?featherlight=false&width=90pc)
4. Click **Save**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-004.png?featherlight=false&width=90pc)
#### Cập nhật Rule
Rule này sẽ chặn tất cả các request thỏa hai điều kiện:
* Chứa header ```x-milkshake: chocolate``` và cả header ```x-favourite-topping: nuts```
* Chứa query ```parameter milkshake=banana``` và cả query parameter ```favourite-topping=sauce```
1. Ở trang thông tin **Web ACL** của bạn.
* Click **Rules**.
* Click **complex-rule-challenge**(tên rule muốn cập nhật).
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-005.png?featherlight=false&width=90pc)
2. Click **Edit**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-006.png?featherlight=false&width=90pc)
3. Trong phần **Rule builder**.
* Click **Rule JSON editor**.
* Tại mục **JSON** điền 
```
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
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-007.png?featherlight=false&width=90pc)
4. Click **Save rule**.
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-008.png?featherlight=false&width=90pc)
5. Click **Save**
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-009.png?featherlight=false&width=90pc)
6. Chạy lệnh
```
# This will be allowed
curl -H "x-milkshake: chocolate" "<Your Juice Shop URL>"
curl  "<Your Juice Shop URL>?milkshake=banana"
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-010.png?width=60pc)
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-011.png?width=60pc)
7. Chạy lệnh
```
# This will be blocked
curl -H "x-milkshake: chocolate" -H "x-favourite-topping: nuts" "<Your Juice Shop URL>"
curl  "<Your Juice Shop URL>?milkshake=banana&favourite-topping=sauce"
```
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-012.png?width=60pc)
![Create Custom Rule](/images/3-useawswaf/3.3-createadvancecustomrule/createadvancecustomrule-013.png?width=60pc)

Các request bị chặn sẽ có phản hồi như sau:
```
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

Ở phần này, bạn đã hiểu được cách để định nghĩa WAF rule ở định dạng JSON. Các logic phức tạp cần phải được định nghĩa sử dụng các toán tử AND, OR và NOT.
