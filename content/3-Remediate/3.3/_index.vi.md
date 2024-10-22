---
title : "Tăng khả năng hiển thị lưu lượng truy cập của Bot"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

### Kịch bản
Nhóm bảo mật của tổ chức bạn nghi ngờ rằng một phần lớn lưu lượng truy cập vào trang web của bạn đến từ nhiều loại bot khác nhau, bao gồm cả bot mong muốn (như công cụ tìm kiếm) và bot không mong muốn (như trình thu thập nội dung). Bạn cần có dữ liệu tốt hơn về số lượng và loại bot trên trang web của mình. Bạn KHÔNG NÊN chặn bất kỳ lưu lượng truy cập nào của bot ngay bây giờ, ít nhất là cho đến khi bạn có cơ hội phân tích tất cả các nguồn lưu lượng truy cập. Tổ chức của bạn cũng muốn giới hạn phạm vi sử dụng kiểm soát bot để kiểm soát chi phí và tránh áp dụng các biện pháp bảo vệ không cần thiết cho các đối tượng tĩnh (bảng định kiểu CSS, tệp JS, v.v.). Các nhà phát triển web đã cung cấp câu lệnh RegEx sau đây khớp với tất cả nội dung tĩnh trên trang web:

```(?i)\.(jpe?g|gif|png|svg|ico|css|js|woff2?)$```

### Hướng dẫn
Bạn có thể sử dụng bộ rule được quản lý của AWS WAF Bot Control để có thể xem được lưu lượng truy cập của bot trên trang web của mình. Nhóm rule được quản lý của Bot Control cung cấp các tùy chọn cho các bot phổ biến và bot mục tiêu. Tùy chọn bot phổ biến sẽ thêm nhãn vào các bot tự nhận dạng, xác minh các bot mong muốn nói chung và phát hiện các chữ ký bot có độ tin cậy cao, bot mục tiêu tập trung vào các bot lan rộng. Để biết thêm chi tiết, vui lòng xem [tài liệu](https://docs.aws.amazon.com/waf/latest/developerguide/waf-bot-control.html).

Tạo một bộ **mẫu regex** khớp với nội dung tĩnh. Thiết lập nhóm rule AWS WAF Bot Control, sử dụng **scope-down** để loại trừ nội dung tĩnh (sử dụng mẫu regex) và đảm bảo rằng nó chỉ đếm lưu lượng truy cập (ghi đè hành vi mặc định). Lưu ý rằng nhóm rule AWS WAF Bot Control vẫn sẽ áp dụng nhãn AWS WAF cho các yêu cầu bot. Bạn sẽ có thể sử dụng các nhãn đó để kiểm soát các yêu cầu ở cấp độ chi tiết.

Sau vài phút, bạn có thể điều hướng đến tab Bot Control để xem thêm thông tin chi tiết về lưu lượng truy cập bot trên trang web của mình.

{{% notice info %}}
Nhiệm vụ này không có bất kỳ bước xác thực bảo vệ nào. Bạn sẽ sử dụng các nhãn do nhóm rule AWS WAF Bot Control áp dụng trong hai nhiệm vụ tiếp theo.
{{% /notice %}}

### Quy trình
#### Bộ mẫu Regex
Đầu tiên, hãy tạo một bộ mẫu regex để lọc nội dung tĩnh dựa trên đầu vào của nhà phát triển.

1. Điều hướng đến các bộ mẫu Regex và xác minh lựa chọn vùng bảng điều khiển WAF là chính xác

![1.1](/images/3/3/regrex_s1.png)
2. có một mẫu regex đã được thiết lập cho bạn”. Nhấp vào đó để biết thêm chi tiết

![1.1](/images/3/3/regrex_s2.png)
Biểu thức chính quy: *(?i)\.(jpe?g|gif|png|svg|ico|css|js|woff2?)$*

### Bộ rule AWS WAF Bot Control
Tiếp theo, hãy bật AWS WAF Bot Control để có thể xem lưu lượng truy cập của bot. Ghi đè các hành động rule để đếm để đảm bảo bạn không chặn bất kỳ lưu lượng truy cập nào của bot. Thao tác này sẽ đếm số lượng kết quả khớp mà không chặn bất kỳ lưu lượng truy cập nào. Sử dụng các câu lệnh scopedown để tránh quét các đối tượng tĩnh khớp với mẫu regex.

#### Bắt đầu thêm AWS WAF Bot Control
1. Mở waf-workshop-webacl trong bảng điều khiển AWS WAF và chọn tab Tổng quan về lưu lượng truy cập

![1.1](/images/3/3/start_s1.png)

2. Trong phần Bộ lọc dữ liệu, chọn tab Bot Control

3. Trong phần Cách thức hoạt động, nhấp vào nhóm rule Thêm AWS WAF Bot Control

![1.1](/images/3/3/regrex_s3.png)
#### Cấu hình Bot Control
1. Đối với cấp độ kiểm tra Bot Control, hãy chọn Common làm cấp độ kiểm tra

![1.1](/images/3/3/bot_s1.png)

2. Trong Bot Control Rules, hãy đặt Override all rule actions thành Count

![1.1](/images/3/3/bot_s2.png)
#### Câu lệnh Scope-down
1. Chọn **Chỉ kiểm tra các yêu cầu khớp với phạm vi-down statement**

2. Câu lệnh Scope-down: enabled (checked)

3. If a request: doesn't match the statement (NOT)

4. Inspect: URI path

5. Match type: Matches pattern from regex patttern set

6. Regex pattern set: static-content (the regex pattern set created above)

7. Text transformation: None

![1.1](/images/3/3/scope_s1.png)

### Đặt mức độ ưu tiên và xem lại các rule
1. Trên trang Đặt mức độ ưu tiên của rule, hãy nhấp vào Lưu (không cần thay đổi)

![1.1](/images/3/3/prio_s1.png)

1. Xác minh rằng bộ rule Bot Control hiện được liệt kê trên tab Rules trong Web ACL

![1.1](/images/3/3/prio_s2.png)

3. Trong vòng vài phút, bạn sẽ có thêm metric về Bot Control tab
![1.1](/images/3/3/prio_s3.png)
4. Tiến hành nhiệm vụ tiếp theo để tận dụng khả năng hiển thị bổ sung do AWS WAF Bot Control cung cấp

Xin chúc mừng! Bạn đã tích hợp thành công AWS WAF Bot Control, tính năng này sẽ bắt đầu áp dụng nhãn cho các yêu cầu bot. Bạn sẽ sử dụng các nhãn đó trong các nhiệm vụ sau.