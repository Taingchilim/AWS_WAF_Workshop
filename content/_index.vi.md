+++
title = "AWS Web Application Firewall"
date = 2020
weight = 1
chapter = false
+++

# AWS WAF là gì

AWS WAF là một dịch vụ web application firewall. Dịch vụ này giúp cho bạn bảo vệ ứng dụng Web hay API của bạn khỏi các tấn công từ bên ngoài gây ảnh hưởng đến vận hành, an ninh thông tin hoặc làm tiêu hao tài nguyên hệ thống.

Sử dụng WAF là một cách để phòng bị cho ứng dụng web của bạn. WAF có thể giảm thiểu các mối nguy hại về lỗ hổng như [SQL Injection](https://www.owasp.org/index.php/SQL_Injection), [Cross Site Scripting](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)) và các tấn công thông dụng khác nằm trong Top 10 OWASP. WAF cho phép bạn tạo các rule tùy chỉnh nhằm cho phép bạn tùy biến việc ngăn chặn hay cho phép các HTTP request từ bên ngoài trước khi chúng tới được ứng dụng của bạn.

#### Nội dung

Nội dung chúng ta sẽ bao gồm các bài thực hành sau:

1. [Các bước chuẩn bị](1-prerequiste/)
2. [Web ACLs và Managed Rules](2-acls-managed-rules/)
3. [Các Rule Tùy chỉnh](3-custom-rules/)
4. [Rule Tùy chỉnh Nâng cao](4-advanced-rules/)
5. [Thử nghiệm các Rule](5-testing/)
6. [Ghi nhận Nhật ký](6-logging/)
7. [Thu hồi tài nguyên](7-clean-up)