---
title : "Bảo vệ API bằng Phân tích nội dung yêu cầu"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 3.6 </b> "
---

### Kịch bản
Cửa hàng web đã xây dựng một API để hỗ trợ tích hợp với các đối tác của họ. API này có thể truy xuất danh sách tối đa 100 sản phẩm cùng một lúc. Nhóm phát triển đang làm việc để thêm các lớp bảo mật vào mã API. Nhiệm vụ của bạn là thêm bảo vệ WAF chỉ cho phép các yêu cầu JSON hợp lệ và giới hạn số lượng sản phẩm được liệt kê ở mức từ 1 đến 100.

Sau đây là một ví dụ về JSON hợp lệ được sử dụng để gọi API này:
```bash
{ "numrecords":"25" }
```
Chỉ các yêu cầu có định dạng JSON hợp lệ và giá trị hợp lệ cho numrecords (bao gồm 1-100) mới được phép. API có sẵn tại đường dẫn **/api/listproducts.php**. Đối với các yêu cầu API không vượt qua tất cả các bước xác thực, rule WAF sẽ trả về mã phản hồi 400 http. Theo RFC 2616, mã phản hồi 400 http cho biết rằng không thể hiểu được yêu cầu và máy khách không nên lặp lại yêu cầu mà không có sửa đổi.

### Hướng dẫn
Đầu tiên, hãy tạo một tập hợp mẫu regex khớp với giá trị hợp lệ (1-100) cho numrecords. Sau đây là một biểu thức chính quy mà bạn có thể sử dụng:
```bash
^0*(?:[1-9][0-9]?|100)$
```
Sau đó, hãy tạo một rule WAF với hai câu lệnh. Một câu lệnh phải khớp với đường dẫn URI của API (/api/listproducts.php). Một câu lệnh khác cần kiểm tra phần thân JSON của các yêu cầu API đến và xác thực rằng cú pháp JSON là hợp lệ. Nó cũng phải xác thực giá trị của "numrecords" bằng cách sử dụng tập hợp mẫu regex. Nếu đường dẫn URI khớp, nhưng cú pháp không đúng, thì rule WAF sẽ trả về phản hồi tùy chỉnh bằng mã phản hồi http 400.

Sau khi triển khai rule WAF mới để bảo vệ chống lại việc lạm dụng API, hãy kiểm tra khả năng bảo vệ của bạn bằng cả quét thủ công và tự động. Đối với quét thủ công, hãy nhấp vào liên kết trong output sự kiện. Đối với quét tự động, hãy kiểm tra Bảng điều khiển tiến trình trong Output sự kiện.

### Quy trình
Quy trình bên dưới dài hơn và phức tạp hơn so với các tác vụ trước. Vui lòng đặc biệt chú ý đến các bước được liệt kê bên dưới.

#### Bộ mẫu biểu thức chính quy
Tạo một bộ mẫu biểu thức chính quy khớp với các số từ 1-100:

1. Điều hướng đến Bộ mẫu biểu thức chính quy và xác minh lựa chọn vùng bảng điều khiển WAF là chính xác

2. Nhấp vào Tạo bộ mẫu biểu thức chính quy

3. Tên bộ mẫu biểu thức chính quy: số-1-đến-100

4. Biểu thức chính quy: **^0*(?:[1-9][0-9]?|100)$**

5. Nhấp vào **Tạo mẫu biểu thức chính quy**

![1.1](/images/3/6/regrex.png)
#### Rule WAF

1. Tạo một rule WAF thông thường khớp với tất cả các câu lệnh:

2. Điều hướng đến tab Rule của Web ACL

3. Nhấp vào Thêm rule và chọn Thêm rule và nhóm rule của riêng tôi

![1.1](/images/3/6/add_rule.png)
#### Chi tiết rule

1. Tên rule: api-protection
2. Type: Rule thông thường
3. Nếu yêu cầu: Phù hợp với tất cả các câu lệnh (AND)

![1.1](/images/3/6/rule_details.png)
#### Câu lệnh #1

1. Inspect: Đường dẫn URI
2. Kiểu khớp: Khớp chính xác với chuỗi
3. Chuỗi cần khớp: /api/listproducts.php
4. Chuyển đổi văn bản: không có

![1.1](/images/3/6/s1.png)
#### Câu lệnh #2

1. Phủ định kết quả câu lệnh (đã chọn)
2. Inspect: Nội dung
3. Kiểu nội dung: JSON
4. Phạm vi khớp JSON: Giá trị
5. AWS WAF nên xử lý yêu cầu như thế nào nếu JSON trong nội dung yêu cầu không hợp lệ: **Không khớp**

![1.1](/images/3/6/s2.png)

So khớp phần tử JSON cụ thể với câu lệnh regex:

1. Nội dung cần kiểm tra: Chỉ các phần tử được bao gồm
2. Được bao gồm elements: /numrecords
3. Match type: Matches pattern from regex pattern set
4. Regex pattern set: (đã tạo trước đó pattern set khớp với các số từ 1-100)
5. Text transformation: None
6. Oversize handling: Continue
![1.1](/images/3/6/s2b.png)
#### Then

1. Action: Block
2. Custom response: Enable (checked)
3. Response Code: 400
4. Click vào Add rule

![1.1](/images/3/6/then.png)
#### Rule priority

1. Trên trang Set rule priority, click vào Save

![1.1](/images/3/6/prio_s1.png)
2. Trên tab Rules, hãy xác minh rằng rule tùy chỉnh mới hiện đã được liệt kê

3. Bạn đã hoàn tất việc thêm rule tùy chỉnh kiểm tra các giá trị JSON trong nội dung yêu cầu và bảo vệ API của bạn khỏi việc sử dụng không đúng cách. Bây giờ bạn đã sẵn sàng để kiểm tra khả năng bảo vệ.

#### Đánh giá hiệu quả bảo vệ
Sử dụng cả phương pháp kiểm tra thủ công và tự động để đánh giá hiệu quả của rule mới trong ACL của bạn:

1. Sử dụng chức năng quét thủ công từ phần Đánh giá. Xác thực rằng bài kiểm tra Sử dụng sai API đang vượt qua.

![1.1](/images/3/6/e_s1.png)
2. Xem lại bảng điều khiển workshop và xác minh rằng quét tự động Sử dụng sai API đang vượt qua (bài kiểm tra phải được mã hóa màu xanh lá cây). Tham khảo phần Đánh giá để biết hướng dẫn.

![1.1](/images/3/6/e_s2.png)