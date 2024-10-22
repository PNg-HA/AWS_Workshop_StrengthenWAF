---
title : "Tạo báo động CloudWatch cho số liệu AWS WAF"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Kịch bản

Tổ chức của bạn lo ngại về việc đối tác sử dụng trình thu thập dữ liệu web và khả năng của trình thu thập dữ liệu trong phạm vi các quy tắc giới hạn tốc độ. Nhiệm vụ của bạn là thiết lập hệ thống cảnh báo sẽ thông báo cho bạn nếu việc sử dụng trình thu thập dữ liệu PHP của họ vượt quá ngưỡng đã thỏa thuận.

#### Hướng dẫn

Đầu tiên, hãy tạo chủ đề SNS không có người đăng ký nào (không cần thiết lập thông báo qua email). Tạo báo động CloudWatch theo dõi quy tắc phpcrawl-rate-limiter và được kích hoạt nếu bất kỳ yêu cầu nào bị chặn trong bất kỳ khoảng thời gian 1 phút nào.

Xác minh cấu hình của bạn bằng cách kích hoạt giới hạn tốc độ. Yêu cầu quá mức của tập lệnh sẽ kích hoạt báo động (thay đổi trạng thái báo động).

#### Quy trình
##### Tạo chủ đề SNS
> **Lưu ý**: Bạn có thể thấy thông báo lỗi tham chiếu đến quyền **KMS**. Bạn có thể bỏ qua thông báo này vì chúng tôi không sử dụng các tính năng mã hóa khi không hoạt động của SNS trong phòng thí nghiệm này.

1. Điều hướng đến Dịch vụ thông báo đơn giản (SNS) trong bảng điều khiển AWS

2. Trong **Tạo chủ đề**, nhập tên **waf-alerts**, sau đó nhấp vào **Bước tiếp theo**

3. Trong **Chi tiết**, nhập thông tin sau:
- Loại: **Chuẩn**
- Tên: **waf-alerts** (phải được điền sẵn)

![1.1](/images/5/1/s3.png)

1. Cuộn xuống và nhấp vào **Tạo chủ đề**

##### Tạo báo thức CloudWatch

1. Điều hướng đến dịch vụ **CloudWatch** trong Bảng điều khiển AWS

2. Điều hướng đến **Báo thức, Tất cả báo thức**

3. Nhấp vào **Tạo báo thức**

![1.1](/images/5/1/alarm_s3.png)

4. Trong "Tạo báo thức", nhấp vào **Chọn metric**

![1.1](/images/5/1/alarm_s4.png)
5. Điều hướng đến **WAFV2, Khu vực, Quy tắc, WebACL**

![1.1](/images/5/1/alarm_s5a.png)
![1.1](/images/5/1/alarm_s5b.png)

6. Chọn số liệu này:
- Quy tắc: **rate-limiter**
- Tên số liệu: **BlockedRequests**

1. Nhấp vào **Chọn số liệu**

![1.1](/images/5/1/select_metric.png)

2. Trong trang tiếp theo, hãy thay đổi **Thời gian** thành **1 phút**

![1.1](/images/5/1/metric_s2.png)

3. Trong "Điều kiện", hãy thay đổi trường **than...** thành **0**, nhấp vào *Tiếp theo*

![1.1](/images/5/1/metric_s3.png)

4. Trong **Thông báo**, nhấp vào *Thêm thông báo* và sử dụng các giá trị sau:
- Kích hoạt trạng thái báo động: **Trong báo động**
- Chọn **Chọn chủ đề hiện có**
- Gửi thông báo đến...: **waf-alerts**

1. Chấp nhận các giá trị mặc định, cuộn xuống và nhấp vào **Tiếp theo**

![1.1](/images/5/1/accept.png)

2. Trong **Tên và mô tả**, nhập **crawler-alarm** làm tên báo động và nhấp vào **Tiếp theo**

![1.1](/images/5/1/accept_s2.png)

3. Trên trang **Xem trước và tạo**, nhấp vào **Tạo báo động**

![1.1](/images/5/1/accept_s3.png)
##### Xác minh cấu hình báo động

Quy trình này sẽ tạo ra một số lượng lớn các yêu cầu để kích hoạt quy tắc WAF giới hạn tỷ lệ, sau đó sẽ công bố số liệu lên CloudWatch. Việc tăng số liệu CloudWatch sẽ kích hoạt báo động.

**Kích hoạt báo động**

1. Đi tới Đầu ra sự kiện và sử dụng liên kết có tên là **Kích hoạt giới hạn tỷ lệ**
2. Tập lệnh sẽ mô phỏng số lượng yêu cầu quá mức đến trang web thử nghiệm của bạn, mô phỏng lưu lượng truy cập tự động
3. Ban đầu, các yêu cầu sẽ dẫn đến phản hồi **200 OK**

![1.1](/images/5/1/final_s3.png)

4. Khi quy tắc giới hạn tỷ lệ được kích hoạt, các yêu cầu sẽ bị chặn và dẫn đến phản hồi **429 Quá nhiều yêu cầu**

![1.1](/images/5/1/final_s4.png)
**Xác minh trong CloudWatch**

> Có thể mất vài phút để Báo động CloudWatch chuyển từ trạng thái "Dữ liệu không đủ" sang trạng thái "Đang báo động".

1. Điều hướng đến dịch vụ **CloudWatch** trong AWS Console
2. Nhấp vào **Trong báo động** để xem các báo động đã kích hoạt
3. Bạn sẽ thấy báo động vừa tạo (**Lưu ý:** có thể mất 1-2 phút để chuyển sang trạng thái trong báo động)

4. Nhấp vào **phpcrawl-alarm** để biết thêm chi tiết với biểu đồ

![1.1](/images/5/1/final.png)

> **Xin chúc mừng!** Báo động giám sát quy tắc AWS WAF của bạn đã được tạo thành công. Điều này đảm bảo bạn sẽ được thông báo bất cứ khi nào quy tắc được kích hoạt.