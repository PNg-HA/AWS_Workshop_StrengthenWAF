---
title : "Chặn Bot xấu bằng AWS Rule tùy chỉnh"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

### Kịch bản
Trong hoạt động trước, bạn đã bật AWS WAF Bot Control để thu thập dữ liệu về hoạt động của bot. Sau khi phân tích hoạt động của bot, bạn đã xác định rằng bot "zyborg" đang gây quá tải cho máy chủ web và không được phép có bot trong ứng dụng của bạn. Bạn cần chặn tất cả các yêu cầu từ bot zyborg.

### Hướng dẫn
Nhóm rule AWS WAF Bot Control thêm nhãn này vào các yêu cầu từ bot zyborg: **awswaf:managed:aws:bot-control:bot:name:zyborg**. Tạo rule WAF với một câu lệnh chặn tất cả các yêu cầu có nhãn này.

Sau khi triển khai rule WAF mới để chặn các yêu cầu của bot zyborg, hãy kiểm tra rule mới của bạn bằng cả quét thủ công và tự động.

### Thủ tục
#### Thêm rule tùy chỉnh vào Web ACL
Điều hướng đến Web ACL và danh sách các rule của nó. Nhấp vào "Thêm rule", chọn "Thêm rule và nhóm rule của riêng tôi".

![1.1](/images/3/4/navigate.png)
##### Chi tiết rule

1. Loại rule: **Trình tạo rule**
2. Tên: **zyborg-block**
3. Loại: **Rule thông thường**
4. Nếu yêu cầu: **phù hợp với câu lệnh**

![1.1](/images/3/4/rule_detail.png)
##### Câu lệnh

1. Kiểm tra: **Có nhãn**
2. Phạm vi khớp: **Nhãn**
3. Khóa khớp: **awswaf:managed:aws:bot-control:bot:name:zyborg**

![1.1](/images/3/4/statement.png)
##### Sau đó

1. Hành động: **Chặn**

2. Nhấp vào **Thêm rule** ở cuối trang

![1.1](/images/3/4/then_1.png)
##### Mức độ ưu tiên của rule

1. Trên trang **Đặt mức độ ưu tiên của rule**, hãy đảm bảo rằng rule zyborg-block mới xuất hiện **DƯỚI** rule do AWS WAF Bot quản lý ("AWS-AWSManagedRulesBotControlRuleSet") và **Lưu**

![1.1](/images/3/4/prio_1.png)

2. Trên tab **Rule**, hãy xác minh rằng rule tùy chỉnh mới hiện đã được liệt kê

![1.1](/images/3/4/prio_2.png)
3. Bạn đã hoàn tất việc thêm rule tùy chỉnh để chặn bot zyborg. Bây giờ bạn đã sẵn sàng để kiểm tra khả năng bảo vệ.

#### Đánh giá hiệu quả bảo vệ

1. Sử dụng chức năng quét thủ công từ phần **Đánh giá**. Xác thực rằng thử nghiệm Bot xấu đang vượt qua và trả về lỗi 403 Cấm đối với yêu cầu đó.
![1.1](/images/3/4/e_s1.png)
2. Xem lại bảng điều khiển workshop và xác minh rằng quét tự động Bad Bot đang vượt qua (màu xanh lá cây). Tham khảo phần Đánh giá để biết hướng dẫn.
![1.1](/images/3/4/e_s2.png)

Xin chúc mừng! Bạn đã bảo vệ thành công trang web của mình khỏi bot zyborg.