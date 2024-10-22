---
title : "Bảo vệ Đường dẫn bằng Rule Tùy chỉnh"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

### Kịch bản
Trang web có thư mục **/includes** với các tệp chỉ được các quy trình trên máy chủ truy cập (ví dụ: tiêu đề và chân trang để hiển thị phía máy chủ). Tuy nhiên, tất cả các tệp trong thư mục đó đều có thể được truy cập trực tiếp từ Internet, điều này có thể dẫn đến tiết lộ thông tin ngoài ý muốn. Nhiệm vụ của bạn là chặn quyền truy cập trực tiếp vào các tệp trong thư mục /includes.

### Hướng dẫn
Tạo rule AWS WAF tùy chỉnh chặn tất cả các yêu cầu bắt đầu bằng /includes. Để ngăn các yêu cầu được mã hóa bỏ qua rule này, hãy thêm chuyển đổi giải mã url trước khi kiểm tra yêu cầu. Sau khi thêm rule tùy chỉnh vào Web ACL, hãy kiểm tra các biện pháp bảo vệ của bạn bằng cả quét thủ công và tự động.

### Quy trình
#### Thêm rule tùy chỉnh vào Web ACL:
1. Điều hướng đến tab Rule của Web ACL

2. Nhấp vào Thêm rule và chọn Thêm rule và nhóm rule của riêng tôi

![1.1](/images/3/2/2.png)
##### Chi tiết rule
1. Loại rule: Trình tạo rule
2. Tên: path-block
3. Loại: Rule thông thường
![1.1](/images/3/2/r1.png)
##### Câu lệnh

1. Nếu yêu cầu: Phù hợp với câu lệnh
2. Kiểm tra: Đường dẫn URI
3. Loại khớp: Bắt đầu bằng chuỗi
4. Chuỗi cần khớp: /includes
5. Chuyển đổi văn bản: Giải mã URL rule thông thường
![1.1](/images/3/2/s1.png)
##### Sau đó

1. Hành động: Chặn
2. Nhấp vào Thêm rule ở cuối trang thông thường rule
![1.1](/images/3/2/t1.png)
##### Mức độ ưu tiên của rule

1. Trên trang Đặt mức độ ưu tiên của rule, hãy nhấp vào Lưu
![1.1](/images/3/2/p1_s1.png)
2. Trên tab Rule, hãy xác minh rằng rule tùy chỉnh mới hiện được liệt kê là rule thông thường
![1.1](/images/3/2/p1_s2.png)
3. Bạn đã hoàn tất việc thêm rule tùy chỉnh để chặn mọi yêu cầu đến các tệp trong thư mục /includes. Bây giờ bạn đã sẵn sàng để kiểm tra khả năng bảo vệ.

#### Đánh giá hiệu quả bảo vệ
Làm theo hướng dẫn để kiểm tra thủ công các biện pháp bảo vệ từ phần Đánh giá để xác thực rằng bài kiểm tra Bao gồm Mô-đun đang vượt qua và trả về lỗi 403 Bị cấm đối với yêu cầu đó.
![1.1](/images/3/2/e1.png)
Xem lại bảng thông tin tiến trình và xác minh rằng các lần quét tự động cho các bài kiểm tra được liệt kê ở trên đang vượt qua. Yêu cầu đến URL có đường dẫn /includes đã bị chặn

![1.1](/images/3/2/e2.png)
Xin chúc mừng! Bạn đã bảo vệ thành công các tệp trong thư mục **/includes**.