+++
title = "Web ACLs với managed rules"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++

#### Tình huống
Giả sử bạn là một lập trình viên đơn độc đang bắt đầu với cửa hàng Juice Shop (Nước Trái Cây). Website của bạn là một ứng dụng web đơn giản chạy với cơ sở dữ liệu SQL. Vì một số lý do nào đó, nhóm tin tặc Milkshake (nhóm Sữa Lắc) đang bắt đầu tấn công vào trang web của bạn.

Rất may là ngay lúc này bạn đang tiếp cận tới AWS WAF. Vậy là bạn quyết định sẽ triển khai WAF để bảo vệ trang web của bạn.

Ngoài ra thì ngay lúc này, bạn đang không có nhiều thời gian nên bạn sẽ quyết định sẽ sử dụng Nhóm các rule dựng sẵn bởi AWS cho Web ACL của bạn. Với các rule này, website của bạn sẽ được bảo vệ khỏi các tấn công thông thường từ phía nhóm Sữa Lắc.
#### Web ACLs với managed rules

{{% notice info %}} 
 [Web ACLs (Web Access Control List)](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) là cốt lỗi của việc triển khai AWS WAF. Nó bao gồm các rule được sử dụng để đánh giá các yêu cầu được gửi tới WAF của bạn. Web ACL được áp dụng lên ứng dụng web của bạn thông qua Amazon CloudFront distribution, AWS API Gateway API hay một AWS Application Load Balancer.\
[Managed rule groups](https://docs.aws.amazon.com/waf/latest/developerguide/waf-managed-rule-groups.html) là một bộ các rule được tạo và cập nhật bởi đội ngũ AWS hoặc các bên thứ 3 trên AWS Marketplace. Những rule này cung cấp khả năng bảo vệ ứng dụng của bạn khỏi các tấn công thông dụng hoặc đặc thù cho từng loại ứng dụng.
{{% /notice %}}

1. Truy cập [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start).
{{% notice note %}} 
Bài thực hành này sử dụng phiên bản mới nhất của AWS WAF. Hãy đảm bảo bạn không sử dụng WAF Classic.
{{% /notice %}}
* Click **Create web ACL**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-001.png?width=90pc)
2. Trong phần **Web ACL details**.
* Tại mục **Resource type**, Click **CloudFront distributions**.
* Tại mục **Name** điền ```waf-workshop-juice-shop```.
* Tại mục **Description** điền ```Web ACL for the aws-waf-workshop```.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-002.png?width=90pc)
3. Trong phần **Associated AWS resources**, Click **Add AWS resources**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-003.png?width=90pc)
4. Trong phần **Add AWS resources**, Click **E24BURECS1O10C - dkievcmqb5kzc.cloudfront.net - WAF Workshop CloudFront Distribution**(CloudFront distribution chúng ta đã tạo).
* Click **Add**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-004.png?width=90pc)
5. Click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-005.png?width=90pc)
6. Trong phần **Rules**.
* Click **Add rules**.
* Click **Add managed rule groups**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-006.png?width=90pc)
7. Tại trang **Add managed rule groups**, Click **AWS managed rule groups**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-007.png?width=90pc)
8. Chọn **Core Rule Set** và **SQL Database**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-008.png?width=90pc)
9. Kéo màn hình xuống dưới, Click **Add rules**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-009.png?width=90pc)
10. Tại trang **Add managed rule groups**, click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-010.png?width=90pc)
11. Tại trang **Set rule priority**, click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-011.png?width=90pc)
12. Tại trang **Configure metrics**, click **Next**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-012.png?width=90pc)
13. Tại trang **Review and create web ACL**, Kéo màn hình xuống dưới, click **Create web ACL**.
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-013.png?width=90pc)
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-014.png?width=90pc)
14. Chạy lệnh
```
# This imitates a Cross Site Scripting attack
# This request should be blocked.
curl -X POST  <Your Juice Shop URL> -F "user='<script><alert>Hello></alert></script>'"
```
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-015.png?width=60pc)
15. Chạy lệnh.
```
# This imitates a SQL Injection attack
# This request should be blocked.
curl -X POST <Your Juice Shop URL> -F "user='AND 1=1;"
```
![Create Web ACL](/images/3-useawswaf/3.1-createwebacl/createwebacl-016.png?width=60pc)
