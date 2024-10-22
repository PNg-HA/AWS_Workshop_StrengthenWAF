---
title : "Chuẩn bị"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

### Tải xuống và thực thi tập lệnh thiết lập

Ta sẽ tải xuống tệp chứa tài nguyên của workshop, mẫu CloudFormation được sử dụng để cung cấp chúng và tập lệnh thiết lập. Sau đó, ta sẽ chạy tập lệnh để triển khai workshop trong tài khoản AWS của riêng bạn.

1. Tạo một thư mục có tên là "waf-workshop".

2. Chạy mã sau (bạn có thể sao chép và dán tất cả các lệnh lại với nhau):

```bash
# Tải xuống tài nguyên của workshop
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/automated-scanner.zip' --output automated-scanner.zip
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/manual-scanner.zip' --output manual-scanner.zip
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/rate-limit-trigger.zip' --output rate-limit-trigger.zip
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/sample-static-website.html' --output sample-static-website.html
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/scanning-dashboard.html' --output scanning-dashboard.html

# Tải xuống mẫu CloudFormation và tập lệnh thiết lập
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/static/waf-workshop.yaml' --output waf-workshop.yaml
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/static/deploy-workshop-own-account.sh' --output deploy-workshop-own-account.sh
```
/2
![1.1](/images/2/2.png)
3. Chạy tập lệnh khởi động trong thiết bị đầu cuối
```bash
sh deploy-workshop-own-account.sh
```
Nếu bạn gặp lỗi *“syntax error: "(" không mong đợi”*, trong khi chạy tập lệnh
![1.1](/images/2/3a.png)
Vui lòng chỉnh sửa dòng đầu tiên của tập lệnh từ “#!/bin/bash” thành “#!/bin/bash” và lưu lại.
![1.1](/images/2/3b.png)
và chạy tập lệnh bằng lệnh sau ```./deploy-workshop-own-account.sh```
![1.1](/images/2/3c.png)

Nếu bạn nhận được thông báo *“Không tạo/cập nhật stack được.”* thì hãy sử dụng lệnh ```aws cloudformation describe-stack-events --stack-name waf-workshop``` hoặc kiểm tra trang Stack trong bảng điều khiển CloudFormation để khắc phục sự cố:
![1.1](/images/2/3d1.png)
![1.1](/images/2/3d2.png)
bạn sẽ thấy rằng bạn phải điều chỉnh MemorySize trong tệp yaml xuống 3008.
![1.1](/images/2/3d3.png)
Sau khi chỉnh sửa stack yaml, hãy chạy lại tập lệnh. Bạn sẽ đợi trong 15 phút sau đó tác vụ được triển khai thành công.
![1.1](/images/2/3e.png)

Vui lòng nhớ xóa tất cả các tài nguyên đã triển khai (stack CloudFormation) sau khi hoàn tất workshop để tránh các khoản phí không cần thiết.

### Đánh giá

Workshop này sẽ hướng dẫn bạn cách đánh giá và cải thiện tình trạng bảo mật của ứng dụng web. Môi trường bạn sẽ làm việc bao gồm một ứng dụng web được lưu trữ trên AWS, được thiết kế có chủ đích để dễ bị tấn công. Mục tiêu của bạn là xác định các lỗ hổng này và triển khai các biện pháp bảo mật để giảm thiểu chúng.

Sau đây là tổng quan về các bước trong phần này:
1. Hiểu kiến ​​trúc: Hiểu kiến ​​trúc đã triển khai và xác định các thành phần cơ sở hạ tầng cơ bản.

2. Truy cập bảng điều khiển tiến trình workshop: Khám phá các phát hiện từ các lần quét tự động của ứng dụng web và tiến trình chung của bạn.

3. Chạy quét thủ công: Thực hiện quét bảo mật thủ công trên ứng dụng web và xác định các thách thức còn lại.

#### Kiến trúc workshop

Kiến trúc của workshop này được hiển thị như bên dưới:
![architecture](/images/waf-workshop-architecture.png)

Đi đến tab Output của trang Stack của trang CloudFormation.

![architecture](/images/2/output.png)

1. Bước đầu tiên là tìm URL trang web trong output có tên 1xWebApplicationURL. Trang web mẫu này đã được triển khai trong tài khoản AWS của bạn cho workshop. Hãy thử mở nó trong một tab mới. Mục tiêu của bạn là bảo mật trang web này bằng AWS WAF.

2. Bạn có thể truy cập bảng điều khiển AWS WAF bằng cách sử dụng liên kết được lưu trữ trong giá trị output 2xWAFConsole. Bạn sẽ sử dụng bảng điều khiển này trong suốt workshop để áp dụng các biện pháp bảo vệ cho ứng dụng web mà bạn vừa xem.

3. Mở liên kết trong 3xProgressDashboard trong một tab khác. Bảng điều khiển này theo dõi tiến trình của bạn trong suốt workshop và tự động cập nhật khi bạn hoàn thành các nhiệm vụ.

4. Bạn cũng có thể kiểm tra thủ công các biện pháp bảo vệ WAF của mình bằng liên kết được cung cấp trong output 4xTestProtections. Điều này sẽ hữu ích sau này trong workshop.

5. 5xTriggerRateLimiting sẽ cho phép bạn kiểm tra tính năng giới hạn tốc độ của AWS WAF sau này trong workshop.

#### Môi trường và công cụ quét trang web
Workshop này cung cấp hai phương pháp để kiểm tra các quy tắc AWS WAF mà bạn sẽ triển khai: quét tự động và quét thủ công. Các trình quét này mô phỏng các vectơ tấn công web phổ biến để giúp bạn xác định và giảm thiểu các rủi ro bảo mật tiềm ẩn, chẳng hạn như:
- SQL Injection (SQLi): Khai thác lỗ hổng để đưa mã độc vào truy vấn cơ sở dữ liệu.
- Cross-site Scripting (XSS): Đưa mã độc vào yêu cầu.
- Path Traversal: Truy cập các tệp hoặc thư mục trái phép trên máy chủ web.
- Sensitive Path Access: Cố gắng truy cập các tệp hoặc thư mục nhạy cảm cần bị hạn chế.
- Bot Activity: Xác định lưu lượng truy cập tự động và chặn bot độc hại.
- API Misuse: Kiểm tra việc sử dụng API không đúng cách.

Các bài kiểm tra này được thiết kế để cung cấp các tình huống chung để đánh giá tiến độ workshop của bạn. Hãy nhớ rằng đối với các triển khai sản xuất, điều quan trọng là phải tiến hành phân tích và thử nghiệm bảo mật kỹ lưỡng ngoài phạm vi của workshop này.

Xin chúc mừng! Bây giờ bạn đã tìm hiểu về kiến ​​trúc được sử dụng trong workshop này, cách tìm output stack có liên quan và những điều cơ bản về các lần quét được thực hiện trên trang web của bạn. Tiếp tục đến phần tiếp theo để tìm hiểu thêm về quá trình quét tự động của ta và cách kích hoạt các lần quét bổ sung theo yêu cầu.

#### Quét tự động
Bảng điều khiển tiến trình cung cấp biểu diễn trực quan về trạng thái bảo mật của môi trường của bạn. Nó hiển thị số lượng các bài kiểm tra tự động (ví dụ: SQL injection, XSS) hiện đang vượt qua cấu hình WAF của bạn. Liên kết để truy cập bảng điều khiển này có thể được tìm thấy trong tab Output của stack CloudFormation.

Lưu ý rằng đôi khi có thể có một chút chậm trễ trong quá trình cập nhật dữ liệu. Nếu bạn tin rằng mình đã hoàn thành một tác vụ nhưng không thấy tác vụ đó trên bảng điều khiển, vui lòng đợi một hoặc hai phút rồi kiểm tra lại.

![dashboard](/images/2/dashboard.png)
Tiếp tục đến phần tiếp theo để tìm hiểu cách kích hoạt quét thủ công các biện pháp bảo vệ ứng dụng web.

#### Quét thủ công
Phần này hướng dẫn bạn quét thủ công điểm cuối ứng dụng web để đánh giá hiệu quả của các quy tắc WAF.

##### Chạy Trình quét thủ công
Truy cập trang Stack và mở liên kết bên dưới output 4xTestProtections trong một tab mới. Thao tác này sẽ thực hiện quét đối với điểm cuối ứng dụng web của bạn. Tập lệnh quét chạy thử nghiệm và báo cáo lại thông tin sau, chẳng hạn như:

Yêu cầu: Lệnh yêu cầu HTTP được sử dụng (ví dụ GET hoặc POST)
Kết quả: Mã trạng thái HTTP được trả về (ví dụ 200 OK hoặc 403 Forbidden)

Kết quả được mã hóa màu để giúp bạn nhanh chóng đánh giá hiệu suất của các quy tắc WAF so với hành vi dự định của chúng. Lý tưởng nhất là tất cả các thử nghiệm sẽ trả về mã trạng thái màu xanh lá cây.

Yêu cầu cơ sở đại diện cho lưu lượng hợp lệ không nên bị các quy tắc WAF của bạn chặn. Chúng sẽ luôn trả về mã trạng thái 200 OK để đảm bảo bạn không vô tình chặn người dùng hợp lệ truy cập vào ứng dụng của mình.

##### Xem lại kết quả quét
Sau khi chạy tập lệnh quét, hãy phân tích kết quả để trả lời những câu hỏi sau:

Có bất kỳ thử nghiệm cơ sở nào bị chặn không? Điều này cho biết các quy tắc WAF của bạn có thể quá hạn chế và cần điều chỉnh.

Các yêu cầu độc hại được mô phỏng có bị chặn thành công không? Điều này cho thấy các quy tắc WAF của bạn đang bảo vệ ứng dụng của bạn một cách hiệu quả.

##### Tự mô phỏng các yêu cầu
Tập lệnh quét tận dụng chức năng AWS Lambda để gửi các yêu cầu thử nghiệm. Bạn có thể sao chép quy trình này bằng cách gửi các yêu cầu tương tự từ thiết bị của riêng bạn bằng các công cụ như curl trên dòng lệnh.

Đối với các bài kiểm tra rất cơ bản, bạn thậm chí có thể gửi các yêu cầu GET đơn giản trực tiếp từ trình duyệt của mình, có khả năng có các tham số chuỗi truy vấn và so sánh các phản hồi bạn nhận được.

Kết quả quét có thể sẽ tiết lộ các lỗ hổng bảo mật cần được giải quyết. Bước tiếp theo, khắc phục, bao gồm việc định cấu hình AWS WAF Web ACL để chặn các yêu cầu độc hại này. Khi AWS WAF chặn thành công một yêu cầu dựa trên các quy tắc của bạn, nó sẽ trả về mã trạng thái 403 Forbidden hoặc mã lỗi tùy chỉnh mà bạn đã xác định.

![manual_scanner](/images/2/manual_scanner.png)
Xin chúc mừng! Bây giờ bạn đã quen với việc xem lại tiến trình của mình và kiểm tra các biện pháp bảo vệ được áp dụng, bạn đã chuẩn bị tốt để chuyển sang phần [Remediate](3-Remediate/). Ở đó, bạn sẽ khám phá các chiến lược để triển khai các biện pháp bảo vệ trang web cần thiết với AWS WAF.