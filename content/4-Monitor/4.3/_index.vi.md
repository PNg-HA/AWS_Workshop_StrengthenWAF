---
title : "Insert Custom HTTP Request Header"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---

#### Kịch bản

Bạn có thể sử dụng tính năng Chèn tiêu đề yêu cầu để giúp xác thực rằng các yêu cầu được gửi đến ứng dụng của bạn đã được AWS WAF đánh giá. Bạn có thể định cấu hình ứng dụng của mình để chỉ cho phép các yêu cầu có chứa tiêu đề tùy chỉnh đã chỉ định. Bạn cũng có thể chèn tiêu đề để ứng dụng của mình có thể xử lý yêu cầu khác nhau dựa trên sự hiện diện của tiêu đề hoặc chỉ cần ghi lại tiêu đề trong nhật ký ứng dụng của bạn để báo cáo và phân tích.

> Không có bước xác minh nào trong bài tập này.

#### Hướng dẫn

Sử dụng tính năng [chèn tiêu đề] yêu cầu (https://docs.aws.amazon.com/waf/latest/developerguide/customizing-the-incoming-request.html) của AWS WAF. Xác minh sự hiện diện của các tiêu đề đã chèn bằng [Nhật ký Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html).

#### Quy trình
##### Chèn Tiêu đề Tùy chỉnh:

1. Điều hướng đến tab **Quy tắc** của Web ACL

2. Nhấp vào **Thêm Quy tắc** và chọn **Thêm quy tắc và nhóm quy tắc của riêng tôi**

![1.1](/images/4/3/s2.png)
**Chi tiết quy tắc**

1. Loại quy tắc: **Trình tạo quy tắc**
2. Quy tắc: **header-insert**, quy tắc thông thường

![1.1](/images/4/3/detail.png)
**Câu lệnh**

1. Nếu yêu cầu: **Phù hợp với câu lệnh**
2. Kiểm tra: **Đường dẫn URI**
3. Loại khớp: **Bắt đầu bằng chuỗi**
4. Chuỗi để khớp: **/api**

![1.1](/images/4/3/statement.png)

**Sau đó**

1. Hành động: **Cho phép**
2. Yêu cầu tùy chỉnh - tùy chọn: Khóa/Giá trị: **request-source, TestBotCanary**
3. Nhấp vào **Thêm quy tắc** ở cuối trang

![1.1](/images/4/3/then.png)
**Mức độ ưu tiên của quy tắc**

1. Trên trang **Đặt mức độ ưu tiên của quy tắc**, nhấp vào **Lưu**
2. Trên tab **Quy tắc**, hãy xác minh rằng quy tắc tùy chỉnh mới hiện đã được liệt kê
3. Bạn đã hoàn tất việc thêm quy tắc tùy chỉnh, hãy chèn tiêu đề tùy chỉnh.

![1.1](/images/4/3/prio.png)
> **Xin chúc mừng!** Bạn đã định cấu hình thành công việc chèn tiêu đề tùy chỉnh.