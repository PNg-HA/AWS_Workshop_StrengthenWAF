---
title : "Tỷ lệ giới hạn lưu lượng truy cập của bot và gửi phản hồi tùy chỉnh"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

### Kịch bản
Sau khi phân tích sâu hơn, nhóm quản lý trang web đã xác định một trong những đối tác của bạn sử dụng bot thu thập thông tin PHP và đôi khi tạo ra các đợt tăng đột biến lưu lượng truy cập, làm chậm trang web đối với những người dùng khác. Đối tác đã được thông báo để làm việc để giảm các đợt tăng đột biến đó. Trong khi bạn chờ họ khắc phục, bạn cần giới hạn tỷ lệ lưu lượng truy cập tự động của họ để tránh bất kỳ tác động nào nữa đến hiệu suất của trang web. Giới hạn tỷ lệ phải là 100 yêu cầu trong 5 phút. Sau khi đạt đến giới hạn, phản hồi phải tuân theo RFC 6585 bằng cách trả về mã lỗi http 429 (quá nhiều yêu cầu) với tiêu đề **Thử lại sau** cho biết người yêu cầu phải đợi 900 giây (15 phút) trước khi thực hiện yêu cầu mới.

Để kiểm tra rule của bạn, hãy chuyển đến tab Stack Outputs trong trang CloudFormation và sử dụng liên kết có tên là Trigger Rate Limiting. Điều đó sẽ tạo ra các yêu cầu quá mức đối với trang web của bạn, mô phỏng lưu lượng truy cập tự động. Sau khi rule giới hạn tỷ lệ được kích hoạt, các phản hồi phải bao gồm mã trạng thái phản hồi HTTP chính xác và tiêu đề chính xác.

### Hướng dẫn
Tạo rule giới hạn tỷ lệ cho phép tối đa 100 yêu cầu trong 5 phút, khớp với nhãn awswaf:managed:aws:bot-control:bot:name:phpcrawl. Rule này sẽ chặn bằng phản hồi tùy chỉnh, mã phản hồi **429**. Chèn khóa tiêu đề tùy chỉnh **Retry-After** và giá trị **900**. Tạo nội dung phản hồi tùy chỉnh có chứa thông báo giải thích lý do chặn: "Chỉ cho phép 100 yêu cầu trong khoảng thời gian 5 phút".

### Quy trình

{{% notice info %}}
Các bước xác minh cho giới hạn tỷ lệ này khác với các tác vụ trước đó. Vui lòng xem xét kỹ các bước xác minh.
{{% /notice %}}

#### Thêm rule tùy chỉnh vào Web ACL:
1. Điều hướng đến tab Rule của Web ACL

2. Nhấp vào Thêm rule và chọn Thêm rule và nhóm rule của riêng tôi

![1.1](/images/3/5/add_rule.png)
#### Chi tiết rule

1. Loại rule: Trình tạo rule
2. Tên: phpcrawl-rate-limiter
3. Loại: Rule dựa trên tỷ lệ

![1.1](/images/3/5/rule_detail.png)
#### Chi tiết tỷ lệ yêu cầu

1. Giới hạn tỷ lệ: 100 trong cửa sổ Đánh giá 5 phút
2. Tổng hợp yêu cầu: Địa chỉ IP nguồn
3. Phạm vi kiểm tra và giới hạn tỷ lệ: Chỉ xem xét các yêu cầu khớp với tiêu chí trong câu lệnh rule

![1.1](/images/3/5/request.png)
#### Chỉ đếm các yêu cầu khớp với các yêu cầu sau statement

1. Nếu yêu cầu: khớp với statement
2. Inspect: Có nhãn
3. Match Key: awswaf:managed:aws:bot-control:bot:name:phpcrawl

![1.1](/images/3/5/count.png)
#### Sau đó

1. Action: Block

2. Custom response: Enable (đã chọn)

3. Response Code: 429

4. Click vào Add new custom header

5. Response Headers:
- Key: Retry-After
- Value: 900

6. Click vào "Add rule"

![1.1](/images/3/5/then.png)
#### Rule priority

1. Trên trang Set rule priority, hãy đảm bảo rằng rule phpcrawl-rate-limiter mới xuất hiện theo thứ tự DƯỚI rule được quản lý AWS WAF Bot Control (AWS-AWSManagedRulesBotControlRuleSet)

2. Click vào Save

3. Trên tab Rule, hãy xác minh rằng rule tùy chỉnh mới hiện đã được liệt kê

4. Bạn đã hoàn tất việc tạo rule tùy chỉnh để giới hạn tốc độ yêu cầu của trình thu thập dữ liệu PHP. Vui lòng lưu ý các bước duy nhất bên dưới để xác thực rule này.

#### Đánh giá hiệu quả bảo vệ
Sử dụng phương pháp thủ công sau để xác thực rule giới hạn tốc độ:

1. Đi tới tab Output stack trong CloudFormation và sử dụng liên kết có tên là Giới hạn tốc độ kích hoạt

2. Tập lệnh sẽ mô phỏng số lượng yêu cầu quá mức đến trang web thử nghiệm của bạn trong khi mô phỏng bot PHPCrawler

3. Ban đầu, các yêu cầu sẽ dẫn đến 200 phản hồi OK

![1.1](/images/3/5/e_s3.png)

4. Khi rule giới hạn tốc độ được kích hoạt, các yêu cầu sẽ bị chặn và dẫn đến 429 phản hồi Quá nhiều yêu cầu

5. Xác nhận rằng tiêu đề Thử lại sau được bao gồm trong các phản hồi

![1.1](/images/3/5/e_s5.png)
Xin chúc mừng! Bạn đã giới hạn tốc độ thành công cho bot PHPCrawler và ngăn chặn các yêu cầu quá mức tới trang web của bạn.