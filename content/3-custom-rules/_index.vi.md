+++
title = "Các Rule Tùy chỉnh"
date = 2020
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

**Nội dung:**
- [Custom Rule](#custom-rule)
- [Tạo Custom Rule](#tạo-custom-rule)
- [Tạo mẫu các Request](#tạo-mẫu-các-request)
- [Web ACL Capacity Units](#web-acl-capacity-units)

#### Custom Rule
WAF cho phép bạn tạo [các rule tùy chỉnh](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) để xử lý các request. Việc này rất hữu ích khi bạn muốn tùy biến theo ngữ cảnh ứng dụng của bạn. Cùng với giới thiệu rule tùy chỉnh, phần này bạn sẽ được làm quèn với việc [tạo mẫu các request](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html#web-acl-testing-view-sample) và [Web ACL Capacity Units](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units).

#### Tạo Custom Rule
Cách đơn giản nhất để tạo các rule tùy chỉnh là sử dụng Trình biên soạn ở trong WAF Console.

![Custom Rule](../../../images/3/1.png?width=90pc)

[Rules](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) cho phép bạn kiểm tra [các thành phần trong một HTTP request](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-fields.html) như:

- Source IP (Địa chỉ IP nguồn)
- Headers (Các header của gói tin)
- Body (Nội dung chính)
- URI (Liên kết được gửi đến)
- Query Parameters (Các thông số truy xuất)

Dựa trên các nội dung mà bạn cần xem xét, bạn có thể chặn hoặc cho phép request đó.

#### Tạo mẫu các Request
WAF cho phép bạn xem các mẫu request đã xử lý. Bạn có thể thực hiện chúng từ Web ACL Dashboard.

![Request Sampling](../../../images/3/2.png?width=90pc)

Đây là công cụ hữu ích cho bạn khi kiểm lỗi nhằm biết được các request được gửi đến là gì và đã được xử lý như thế nào.

Ngoài ra chúng còn ghi nhận lại tất cả các request mà Web ACL của bạn nhận được. Chúng ta sẽ đề cập với nội dung này sau.

#### [Web ACL Capacity Units](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units)

{{% notice tip %}}
Web ACL của bạn có tối đa 1500 WCU. Hạn mức này có thể được tăng thêm thông qua việc gửi yêu cầu đến AWS Support.
{{% /notice %}}

Bạn có thể tham khảo thêm ở tài liệu [Thông tin chi tiết về WCU](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units)
