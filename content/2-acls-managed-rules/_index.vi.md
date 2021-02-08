+++
title = "Web ACLs and Managed Rules"
date = 2020
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

**Nội dung:**
- [Web ACLs](#web-acls)
- [Các Rule Dựng sẵn](#các-rule-dựng-sẵn)

#### Web ACLs
[Web ACL (Web Access Control List)](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) là cốt lỗi của việc triển khai AWS WAF. Nó bao gồm các rule được sử dụng để đánh giá các yêu cầu được gửi tới WAF của bạn. Web ACL được áp dụng lên ứng dụng web của bạn thông qua Amazon CloudFront distribution, AWS API Gateway API hay một AWS Application Load Balancer.

{{% notice tip %}}
Bài thực hành này sử dụng phiên bản mới nhất của AWS WAF. Hãy đảm bảo bạn không sử dụng WAF Classic.
{{% /notice %}}

#### Các Rule Dựng sẵn
Cách nhanh nhất để bắt đầu với WAF là triển khai [Nhóm các Rule dựng sẵn cho AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-list.html) cho Web ACL của bạn.

[Nhóm các Rule dựng sẵn](https://docs.aws.amazon.com/waf/latest/developerguide/waf-managed-rule-groups.html) là một bộ các rule được tạo và cập nhật bởi đội ngũ AWS hoặc các bên thứ 3 trên AWS Marketplace. Những rule này cung cấp khả năng bảo vệ ứng dụng của bạn khỏi các tấn công thông dụng hoặc đặc thù cho từng loại ứng dụng.

Mỗi nhóm các rule dựng sẵn sẽ có thể bảo vệ ứng dụng khói một tập các tấn công thông thường. Ví dụ như các tấn công đến SQL hay Command Line.

AWS cung cấp nhiều sự lựa chọn cho nhóm các rule dựng sẵn. Điển hình như ba loại *Amazon IP Reputation list*, *Known Bad Inputs* và *Core rule set*. Ngoài ra còn những nhóm các rule khác có thể được sử dụng.
