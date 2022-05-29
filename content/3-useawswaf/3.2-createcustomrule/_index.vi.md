+++
title = "Custom Rule"
date = 2020
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++
#### Tình huống
Ngay khi bạn nghĩ là bạn đã ngăn chặn được các tấn công từ nhóm Sữa Lắc, lại có thêm các request tấn công vào ứng dụng của bạn. Các tấn công này ngày càng nguy hiểm hơn. Tuy nhiên bạn có thể tự tin có thể chặn các tấn công này bằng các rule tùy biến cho WAF Web ACL của bạn.

Tất cả các tấn công có vẻ như đều có các header lạ ```X-TomatoAttack``` và bạn cần ngăn chặn các request có header này sẽ chặn được đợt tấn công này.
#### Tạo Custom Rule
{{% notice info %}} 
WAF cho phép bạn tạo [các rule tùy chỉnh](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) để xử lý các request. Việc này rất hữu ích khi bạn muốn tùy biến theo ngữ cảnh ứng dụng của bạn. Cùng với giới thiệu rule tùy chỉnh, phần này bạn sẽ được làm quèn với việc [tạo mẫu các request](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html#web-acl-testing-view-sample) và [Web ACL Capacity Units](https://docs.aws.amazon.com/waf/latest/developerguide/how-aws-waf-works.html#aws-waf-capacity-units).
{{% /notice %}}
1. Ở trang thông tin **Web ACL** của bạn.
* Click **Rules**.
* Click **Add Rules**.
* Click **Add my own rules and rule groups**.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-001.png?width=90pc)
2. Trong phần **Rule builder**.
* Tại mục **Name** điền ```MyCustomRule-X-TomatoAttack```.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-002.png?width=90pc)
3. Trong phần **Statement**.
* Tại mục **Inspect** Chọn **Single header**.
* Tại mục **Header field name** điền ```X-TomatoAttack```.
* Tại mục **Match type** Chọn **Size greater than or equal to**.
* Tại mục **Size in bytes** điền ```0```.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-003.png?width=90pc)
4. Trong phần **Action**.
* Tại mục **Action** Click **Block**.
* Click **Add rule**.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-004.png?width=90pc)
5. Click **Save**.
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-005.png?width=90pc)
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-006.png?width=90pc)
{{% notice info %}} 
Bạn có thể đạt được kết quả tương tự nếu dùng [regular expression](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-regex-pattern-set-match.html)
{{% /notice %}}
6. Chạy lệnh.
```
# This will be blocked
curl -H "X-TomatoAttack: Red" "<Your Juice Shop URL>"
```
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-007.png?width=60pc)
7. Chạy lệnh.
```
# This will be blocked
curl -H "X-TomatoAttack: Green" "<Your Juice Shop URL>"
```
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-008.png?width=60pc)
8. Ở trang thông tin **Web ACL** của bạn.
* Click **Overview**.
* Kéo màn hình xuống phần **Sampled requests**, Bạn sẽ thấy các request nhận được có trạng thái **BLOCK**..
![Create Custom Rule](/images/3-useawswaf/3.2-createcustomrule/createcustomrule-009.png?width=90pc)