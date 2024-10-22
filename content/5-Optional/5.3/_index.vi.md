---
title : "Bảo vệ biểu mẫu web bằng CAPTCHA"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Kịch bản

Trang web mẫu được triển khai như một phần của hội thảo này có chứa Bài kiểm tra. Vui lòng thêm bảo vệ CAPTCHA AWS WAF vào trang Bài kiểm tra để đảm bảo rằng câu trả lời đến từ người dùng hợp lệ.

#### Hướng dẫn

Sử dụng [AWS WAF CAPTCHA](https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge-best-practices.html) để đảm bảo rằng chỉ có con người mới có thể truy cập biểu mẫu được lưu trữ tại /quiz. Xác thực thủ công rằng quy tắc đang hoạt động. Giải câu đố CAPTCHA được trình bày và giải Bài kiểm tra AWS WAF.

#### Quy trình
##### Thêm quy tắc tùy chỉnh vào Web ACL:

1. Điều hướng đến tab **Quy tắc** của Web ACL

2. Nhấp vào **Thêm quy tắc** và chọn **Thêm quy tắc và nhóm quy tắc của riêng tôi**

![1.1](/images/5/3/s1.png)
**Chi tiết quy tắc**

1. Loại quy tắc: **Trình tạo quy tắc**
2. Tên: **quiz-captcha**
3. Loại: **Quy tắc thông thường**

![1.1](/images/5/3/rule_detail.png)

**Câu lệnh**

1. Nếu yêu cầu: **Khớp với câu lệnh**
2. Kiểm tra: **Đường dẫn URI**
3. Loại khớp: **Khớp chính xác với chuỗi**
4. Chuỗi cần khớp: **/quiz**
5. Chuyển đổi văn bản: **Không có**

![1.1](/images/5/3/statement.png)

**Sau đó**

1. Hành động: **CAPTCHA**
2. Kiểm tra **Đặt thời gian miễn trừ tùy chỉnh cho quy tắc này**
3. Thời gian miễn trừ: **900** giây
4. Nhấp vào **Thêm quy tắc** ở cuối trang

![1.1](/images/5/3/then.png)

**Mức độ ưu tiên của quy tắc**

1. Trên trang **Đặt mức độ ưu tiên của quy tắc**, nhấp vào **Lưu**
2. Trên tab **Quy tắc**, hãy xác minh rằng quy tắc tùy chỉnh mới hiện đã được liệt kê (có thể nằm trên trang thứ 2 của các quy tắc)

![1.1](/images/5/3/prio_s2.png)

3. Bạn đã hoàn tất việc thêm quy tắc tùy chỉnh bằng CAPTCHA chỉ cho phép con người truy cập. Bây giờ bạn đã sẵn sàng để kiểm tra khả năng bảo vệ.

##### Đánh giá hiệu quả bảo vệ

1. Điều hướng đến ứng dụng web mẫu.

2. Mở **Bài kiểm tra AWS WAF** bằng liên kết ở góc trên bên phải.

![1.1](/images/5/3/e_s2.png)

3. Sau đó, bạn sẽ thấy một trang yêu cầu bạn xác nhận rằng bạn là người. Giải câu đố được hiển thị cho bạn.

![1.1](/images/5/3/e_s3.png)

4. Bạn sẽ thấy thông báo **Thành công** trong giây lát và được chuyển hướng đến trang Bài kiểm tra.

5. Giải Bài kiểm tra và nhấp vào nút **Gửi**. Bạn sẽ thấy thông báo bật lên với kết quả Bài kiểm tra.

![1.1](/images/5/3/e_s5.png)

> **Xin chúc mừng!** Bạn đã triển khai thành công bảo vệ **AWS WAF CAPTCHA** cho biểu mẫu web.