+++
title = "Thử thách ACL và Rule"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++

**Contents:**
- [Thử thách](#thử-thách)
- [Cấu hình: Tạo custome rule](#cấu-hình-tạo-custome-rule)
- [Kiểm thử hoạt động](#kiểm-thử-hoạt-động)

#### Thử thách
Ngay khi bạn nghĩ là bạn đã ngăn chặn được các tấn công từ nhóm Sữa Lắc, lại có thêm các request tấn công vào ứng dụng của bạn. Các tấn công này ngày càng nguy hiểm hơn. Tuy nhiên bạn có thể tự tin có thể chặn các tấn công này bằng các rule tùy biến cho WAF Web ACL của bạn.

Tất cả các tấn công có vẻ như đều có các header lạ ```X-TomatoAttack``` và bạn cần ngăn chặn các request có header này sẽ chặn được đợt tấn công này.

{{%expand "Chọn vào đây để xem gợi ý" %}}
- Tạo một custome rule
- Request được gửi đến có một header cụ thể.
- Độ dài header này là bao nhiêu?
{{% /expand%}}

#### Cấu hình: Tạo custome rule

Tạo một rule trong Web ACL dùng để chặn các request với header ```X-TomatoAttack``` với bất kì giá trị nào.

{{%expand "Chọn để xem các bước thực hiện" %}}
1. Ở trang thông tin **Web ACL** của bạn, chọn tab **Rules**.
2. Chọn **Add Rules** > **Add my own rules and rule groups**.

![ACLs and Rules Challenge](../../../images/3/1.png?width=90pc)

3. Ở trang **Add rule**, sử dụng **Rule builder** để xây dựng custom rule của bạn:
   - Ở mục Rule:
     - **Name**:  Nhập tên rule (VD: MyCustomRule-X-TomatoAttack)
     - **Type**:  Regular rule
   - **If a request**:  matches the statement
   - Ở mục **Statement**:
     - **Inspect**: Header
     - **Header field name**: ```X-TomatoAttack```
     - **Match type**:  Size greater or equal to
     - **Size**:  0
   - Ở mục **Action**:  Chọn Block
   - Chọn **Add Rule** > **Save** để thêm Rule vào Web ACL.

![ACLs and Rules Challenge](../../../images/3/2.png?width=90pc)

![ACLs and Rules Challenge](../../../images/3/3.png?width=90pc)

![ACLs and Rules Challenge](../../../images/3/4.png?width=90pc)
{{% /expand%}}

Bạn có thể đạt được kết quả tương tự nếu dùng [regasdular expression](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-regex-pattern-set-match.html).

Nếu Rule của bạn không hoạt động, hãy kiểm tra các thông tin sau:
1. Web ACL đã liên kết đến **Application Load Balancer** hay chưa?
2. Giá trị **Header field name** có được nhập chính xác hay chưa?

#### Kiểm thử hoạt động

Bước kiểm thử này sẽ thực hiện gửi request đến ứng dụng của bạn. Nếu WAF rule hoạt động, request sẽ bị chặn lại và bạn sẽ nhận được thông báo 403 như bên dưới.

```html
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

Trong cửa sổ lệnh, chạy lệnh sau:

```bash
# Set the JUICESHOP_URL variable if not already done
# JUICESHOP_URL=<Your Juice Shop URL>

# This should be blocked
curl -H "X-TomatoAttack: Red" "${JUICESHOP_URL}"
```

![ACLs and Rules Challenge](../../../images/3/5.png?width=90pc)

```bash
# This should be blocked
curl -H "X-TomatoAttack: Green" "${JUICESHOP_URL}"
```

![ACLs and Rules Challenge](../../../images/3/6.png?width=90pc)

Kiểm tra thông tin [WebACL overview to see the sampled requests](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html#web-acl-testing-view-sample#web-acl-testing-view-sample). Bạn sẽ thấy các request nhận được có trạng thái **BLOCK**.

![ACLs and Rules Challenge](../../../images/3/7.png?width=90pc)

Khi hoàn tất nghĩa là custom rule của bạn đã hoạt động.

Custom rule cho phép bạn tùy chỉnh logic của bạn để xử lý các request trong WAF. Custom rule có thể xem xét và đánh giá nhiều thành phần của request và sau đó thực hiện chặn hoặc cho phép request đó nếu chúng đúng theo định nghĩa của rule.

Mỗi Web ACL có giới hạn là 1500 Web ACL Capacity Units (WCU) và giới hạn này có thể tăng lên thông qua yêu cầu đến AWS Support. Mỗi rule và nhóm rule trong Web ACL sẽ được tính vào giới hạn này.
