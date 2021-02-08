+++
title = "Web ACLs and Managed Rules"
date = 2020
weight = 2
chapter = false
pre = "<b>2. </b>"
+++

**Nội dung:**
- [Thử thách của bạn](#thử-thách-của-bạn)
- [Cấu hình: Tạo Web ACL](#cấu-hình-tạo-web-acl)
- [Cấu hình: Thêm Nhóm rule dựng sẵn](#cấu-hình-thêm-nhóm-rule-dựng-sẵn)
- [Kiểm thử hoạt động](#kiểm-thử-hoạt-động)

#### Thử thách của bạn
Giả sử bạn là một lập trình viên đơn độc đang bắt đầu với cửa hàng Juice Shop (Nước Trái Cây). Website của bạn là một ứng dụng web đơn giản chạy với cơ sở dữ liệu SQL. Vì một số lý do nào đó, nhóm tin tặc Milkshake (nhóm Sữa Lắc) đang bắt đầu tấn công vào trang web của bạn.

Rất may là ngay lúc này bạn đang tiếp cận tới AWS WAF. Vậy là bạn quyết định sẽ triển khai WAF để bảo vệ trang web của bạn.

Ngoài ra thì ngay lúc này, bạn đang không có nhiều thời gian nên bạn sẽ quyết định sẽ sử dụng Nhóm các rule dựng sẵn bởi AWS cho Web ACL của bạn. Với các rule này, website của bạn sẽ được bảo vệ khỏi các tấn công thông thường từ phía nhóm Sữa Lắc.

#### Cấu hình: Tạo Web ACL

**Tạo Web ACL từ WAF console.**

{{%expand "Chọn để xem các bước thực hiện" %}}
1. Truy cập [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/start).
2. Chọn **Create web ACL** để bắt đầu. Nhập các thông tin cho Web ACL của bạn:.
3. Set the **Region** to the **Global (CloudFront)**.
   - **Name**:  ```waf-workshop-juice-shop```
   - **CloudWatch metric name**:  Giữ giá trị mặc định. (VD: waf-workshop-juice-shop)
   - **Description**: ```Web ACL for the aws-waf-workshop```.
   - **Resource type**: CloudFront distributions

![CloudFormation](../../../images/2/1.png?width=90pc)

4. Trong mục **Associated AWS resources** chọn **Add AWS resources**.

![CloudFormation](../../../images/2/2.png?width=90pc)

5. Chọn **CloudFront distribution**.

![CloudFormation](../../../images/2/3.png?width=90pc)
{{% /expand%}}

**Liên kết Web ACL vào CloudFront distribution của bạn**

{{%expand "Chọn để xem các chú ý" %}}
Nếu CloudFront distribution không hiển thị khi bạn thiết lập liên kết, hãy kiểm tra:
1. Thiết lập **Resource type** bạn đã chọn **CloudFront Distribution** hay chưa?
2. CloudFormation template ở [Phần 1 - Các bước chuẩn bị](../../1-prerequiste/#triển-khai-một-ứng-dụng-mẫu) đã được triển khai hoàn tất hay chưa?
{{% /expand%}}

#### Cấu hình: Thêm Nhóm rule dựng sẵn

Bạn sẽ thêm hai nhóm rule vào Web ACL.

{{%expand "Chọn để xem các bước thực hiện" %}}
1. Ở trang thông tin **Web ACL** bạn đã tạo, chọn tab **Rules**.
2. Chọn **Add Rules** > **Add Managed Rule Groups**.

![CloudFormation](../../../images/2/4.png?width=90pc)

3. Chọn **Core Rule Set** và **SQL Database** từ lựa chọn **AWS managed rule groups**.

![CloudFormation](../../../images/2/5.png?width=90pc)

![CloudFormation](../../../images/2/6.png?width=90pc)

4. Chọn **Add rules** để thêm vào Web ACL của bạn.
5. Chọn **Next** ở **Step 3** và **Step 4**. Sau đó chọn **Create web ACL** ở **Step 5** để tạo Web ACL.

![CloudFormation](../../../images/2/7.png?width=90pc)

![CloudFormation](../../../images/2/8.png?width=90pc)
{{% /expand%}}

#### Kiểm thử hoạt động

Kiểm thử rule mới của bạn với lệnh sau:

{{% notice info %}}
Hãy đảm bảo bạn đã thiết lập biến ```JUICESHOP_URL``` với đường dẫn đến Juice Shop của bạn
{{% /notice %}}

```bash
export JUICESHOP_URL=<Your Juice Shop URL>
```

```bash
# This imitates a Cross Site Scripting attack
# This request should be blocked.
curl -X POST  $JUICESHOP_URL -F "user='<script><alert>Hello></alert></script>'"
```

![CloudFormation](../../../images/2/9.png?width=90pc)

```bash
# This imitates a SQL Injection attack
# This request should be blocked.
curl -X POST $JUICESHOP_URL -F "user='AND 1=1;"
```

![CloudFormation](../../../images/2/10.png?width=90pc)

- Nếu request bị chặn, bạn sẽ nhận lại một phải hồi với tình trạng bị ngăn chặn. Nội dung sẽ tương tự như bên dưới.

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

- Nếu nội dung trả về như sau nghĩa là yêu cầu chưa bị chặn bởi WAF.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>OWASP Juice Shop</title>
    // Omitted for brevity
  </head>
</html>
```

Trong trường hợp này, đã có thiết lập chưa đúng. Hãy kiểm tra lại các thông tin sau:
- Web ACL của bạn đã liên kết với CloudFront distribution chưa?
  - Nếu chưa, CloudFront distribution sẽ không được bảo vệ bởi các rule thiết lập trong Web ACL.
- Web ACL của bạn đã áp dụng các nhóm rule như ở trên (Core Rule Set và SQL Database) chưa?
  - Nếu các nhóm rule chưa được áp dụng thì Web ACL của bạn sẽ không ngăn chặn các request được gửi tới.
