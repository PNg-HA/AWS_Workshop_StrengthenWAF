---
title : "Review the WAF Bot Dashboard"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Kịch bản

Nếu bạn đã bật rule phát hiện Bot AWS WAF trong bài tập trước đó, bảng điều khiển Bot Control của bạn sẽ chứa dữ liệu thú vị để phân tích. Xem lại bảng điều khiển để phân tích hoạt động của bot trên trang web của bạn.

#### Hướng dẫn

Trong WebACL của bạn, hãy điều hướng đến tab Bot Control và xem lại dữ liệu trong 1 giờ qua. Sử dụng bảng điều khiển để có cái nhìn sâu sắc nhanh về lưu lượng truy cập của bot trên trang web của bạn:
- Tỷ lệ phần trăm lưu lượng truy cập là bot là bao nhiêu?
- Có bao nhiêu bot được phép truy cập trong 5 phút qua? Có bao nhiêu bot bị chặn?
- Loại bot hàng đầu là gì?
- Bot **Zyborg** đã cố gắng truy cập trang web của bạn bao nhiêu lần trong 5 phút qua?

> Không có bước xác thực hoặc xác minh nào trong bài tập này.

#### Quy trình
##### Điều hướng đến bảng điều khiển Bot Control

1. Điều hướng đến WebACL trong bảng điều khiển AWS WAF

2. Chọn tab **Bot Control**

![bot_dashboard](/images/5/2/bot_dashboard.png)

3. Bạn sẽ thấy các biểu đồ tương tự như ảnh chụp màn hình
![bot_dashboard](/images/5/2/bot-dashboard_2.png)

##### Thu thập metric

1. Kiểm tra dữ liệu trong **Tất cả các yêu cầu, theo phần trăm**

2. Ảnh chụp màn hình bên dưới hiển thị phần có hình chữ nhật màu đỏ

3. Nhấp vào **1h** ở góc trên bên phải của bảng điều khiển

![1.1](/images/5/2/s2.png)

4. Biểu đồ sẽ hiển thị dữ liệu theo khoảng thời gian 5 phút

5. Xem lại số lượng yêu cầu được phép so với số lượng yêu cầu bị chặn trong vòng 5 phút qua interval

![1.1](/images/5/2/s5.png)

6. Biểu đồ tiếp theo hiển thị các danh mục bot hàng đầu

![1.1](/images/5/2/s6.png)
##### Thu thập dữ liệu trên một bot cụ thể

1. Trong danh sách thả xuống **Duyệt hoạt động của bot**, chọn **Zyborg.**
2. Biểu đồ bên dưới sẽ hiển thị dữ liệu cho bot Zyborg (tương tự như ảnh chụp màn hình này):

![1.1](/images/5/2/collect_data_s2.png)
> **Xin chúc mừng!** Bạn đã học thành công cách phân tích nhanh lưu lượng bot trong bảng điều khiển **Kiểm soát bot**.