---
title : "Áp dụng Bảo vệ Nền tảng với AWS Managed Rule"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

### Kịch bản
Nhóm bảo mật của bạn đã xếp hạng bảo vệ chống lại các cuộc tấn công phổ biến, chẳng hạn như Cross-Site Scripting (XSS), path traversal và SQL injection (SQLi) là ưu tiên cao. Nhiệm vụ của bạn là sử dụng AWS WAF để giảm rủi ro của các cuộc tấn công ứng dụng web phổ biến này vào trang web của bạn. Tổ chức của bạn muốn có một giải pháp được quản lý hoàn toàn, không yêu cầu tạo quy tắc thủ công.

### Hướng dẫn
Xem lại Web ACL đã được định cấu hình cho ứng dụng web của bạn. Thêm **Bộ quy tắc cốt lõi** và nhóm quy tắc được quản lý **cơ sở dữ liệu SQL** vào WebACL. Nhóm quy tắc Bộ quy tắc cốt lõi (CRS) chứa các quy tắc thường áp dụng cho các ứng dụng web. Điều này cung cấp khả năng bảo vệ chống lại việc khai thác nhiều lỗ hổng khác nhau, bao gồm các lỗ hổng rủi ro cao và thường xảy ra được mô tả trong các ấn phẩm của OWASP như OWASP Top 10. Nhóm quy tắc cơ sở dữ liệu SQL chứa các quy tắc để chặn các mẫu yêu cầu liên quan đến việc khai thác cơ sở dữ liệu SQL, như các cuộc tấn công tiêm SQL. Xem lại [tài liệu AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-list.html) để biết thêm chi tiết về các quy tắc có trong bộ quy tắc Core và nhóm quy tắc cơ sở dữ liệu SQL.

Sau khi triển khai bộ quy tắc Core và nhóm quy tắc Cơ sở dữ liệu SQL, hãy kiểm tra các biện pháp bảo vệ của bạn bằng cả quét thủ công và quét tự động. Sử dụng máy quét thủ công hoặc kiểm tra **Bảng điều khiển WAF** để quét tự động.

### Quy trình
#### Thêm các quy tắc được quản lý vào WAF ACL
Điều hướng đến WAF ACL và tab Quy tắc của nó:

1. Mở [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/web-acls?region=global).

2. Xác minh rằng bạn đang sử dụng vùng Toàn cầu (CloudFront)

![2](/images/3/1/2.png)
1. Mở WebACL có tên là waf-workshop-webacl

2. Chọn tab Quy tắc

Thêm bộ quy tắc Lõi và nhóm quy tắc được quản lý của cơ sở dữ liệu SQL:

1. Nhấp vào Thêm Quy tắc và chọn Thêm nhóm quy tắc được quản lý
![1.1](/images/3/1/1.png)

2. Mở rộng Nhóm quy tắc được quản lý của AWS

3. Nhấp vào công tắc cho bộ quy tắc Lõi (đổi màu thành màu xanh lam)

![1.1](/images/3/1/4a.png)
1. Nhấp vào công tắc cho cơ sở dữ liệu SQL (đổi màu thành màu xanh lam)
![1.1](/images/3/1/4b.png)
1. Nhấp vào Thêm quy tắc ở cuối trang

6. Trên trang Đặt mức độ ưu tiên của quy tắc, nhấp vào Lưu

7. Trên tab Quy tắc, hãy xác minh rằng AWS Managed Common và SQLi Rule Sets đều có mặt

8. Bạn đã thêm hai bộ quy tắc được quản lý vào WebACL. Bây giờ bạn đã sẵn sàng để kiểm tra chúng.

### Đánh giá hiệu quả bảo vệ
Thực hiện theo hướng dẫn để kiểm tra thủ công các biện pháp bảo vệ từ phần Đánh giá để xác thực rằng các bài kiểm tra SQL injection, Cross-site scripting (XSS) và Path traversal đang vượt qua.
![1.1](/images/3/1/e1.png)
Xem lại bảng thông tin tiến trình và xác minh rằng các lần quét tự động cho các bài kiểm tra được liệt kê ở trên đang vượt qua.
![1.1](/images/3/1/e2.png)
Xin chúc mừng! Bạn đã sử dụng thành công bộ quy tắc Core và bộ quy tắc cơ sở dữ liệu SQL để áp dụng các biện pháp bảo vệ cơ bản chống lại các cuộc tấn công ứng dụng web!