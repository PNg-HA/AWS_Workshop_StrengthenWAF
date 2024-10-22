---
title : "Block Mystery Test"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2. </b> "
---

#### Kịch bản

Một số yêu cầu từ máy quét tự động bao gồm tiêu đề `mystery-hint`. Nhiệm vụ của bạn là tìm giá trị của tiêu đề đó, giải mã nó và chặn các yêu cầu trong tương lai có cùng giá trị.

#### Hướng dẫn

Sử dụng Amazon Athena để khám phá nhật ký AWS WAF và tìm giá trị của tiêu đề `mystery-hint` trong các yêu cầu. Giá trị được mã hóa base64, vì vậy bạn cần giải mã văn bản để tìm giá trị. Xây dựng quy tắc WAF tùy chỉnh để chặn giá trị tiêu đề mystery-hint bằng câu lệnh thích hợp (xem xét chuyển đổi văn bản có thể cần thiết).

Xác thực quy tắc của bạn bằng cách xem lại bảng điều khiển kiểm tra tự động mà quá trình quét tự động **MysteryTest** đang vượt qua. Nếu bạn đã hoàn thành các tác vụ khắc phục, thì việc hoàn thành bài tập này sẽ chuyển ô cuối cùng trên tab **Tiến trình của tôi** của Bảng điều khiển WAF sang **màu xanh lá cây!**

#### Quy trình
##### Chuyển sang Nhóm làm việc Amazon Athena

Bạn có thể bỏ qua phần này nếu bạn đã đăng nhập vào bảng điều khiển Amazon Athena và đã chuyển sang nhóm làm việc **waf-workshop**.

**Chuyển sang nhóm làm việc waf-workshop Athena**

1. Điều hướng đến [Bảng điều khiển Amazon Athena](https://console.aws.amazon.com/athena)

2. Trong danh sách thả xuống nhóm làm việc, hãy chọn tên nhóm làm việc **waf-workshop**

3. Nếu hiển thị cửa sổ bật lên cài đặt, hãy nhấp vào **Xác nhận**

##### Tìm giá trị của Mystery Hint

Sử dụng truy vấn Amazon Athena để phân tích cú pháp nhật ký AWS WAF và tìm giá trị của tiêu đề `mystery-hint`.

1. Mở tab **Saved queries** và nhấp vào truy vấn **MysteryHintHeader**.

![1.1](/images/4/2/find_s1.png)

2. **Chạy truy vấn** để lấy giá trị được mã hóa của tiêu đề. Lưu ý giá trị được mã hóa [Base64](https://en.wikipedia.org/wiki/Base64) của tiêu đề `mystery-hint`

![1.1](/images/4/2/find_s2.png)
3. Bạn có thể sử dụng bất kỳ phương pháp nào để giải mã giá trị; nếu bạn cần trợ giúp, các bước trong phần tiếp theo sẽ chỉ cho bạn cách sử dụng các hàm tích hợp của Amazon Athena để thực hiện giải mã. Trong bước này, hãy sử dụng https://www.base64decode.org/, nhận dạng giá trị đã giải mã

![1.1](/images/4/2/find_s3.png)
##### Giải mã giá trị của Mystery Hint

> Để giải mã giá trị tiêu đề, truy vấn sau sử dụng các hàm Athena from_base64() và from_utf8(). Bạn có thể đọc thêm về các hàm được hỗ trợ trong [Hướng dẫn sử dụng Athena](https://docs.aws.amazon.com/athena/latest/ug/functions.html).

1. Nhấp vào biểu tượng + để bắt đầu truy vấn Amazon Athena mới

2. Viết lại truy vấn đã lưu trước đó **MysteryHintHeader** bằng cách sao chép nó sang một truy vấn mới với tra cứu bổ sung về giá trị đã giải mã của tiêu đề `mystery-hint`:

```
from_utf8(from_base64('<giá trị đã mã hóa>'))
```

Nếu bạn cần trợ giúp về truy vấn, vui lòng tìm gợi ý bên dưới:

**Gợi ý truy vấn**

```
WITH DATASET AS (SELECT header FROM waf_access_logs CROSS JOIN UNNEST(httprequest.headers) AS t(header))
SELECT DISTINCT header.name, header.value as encoded_header_value, from_utf8(from_base64(header.value)) as decoded_header_value
FROM DATASET
WHERE LOWER(header.name)='mystery-hint'
```

3. Chạy truy vấn để lấy giá trị tiêu đề

![1.1](/images/4/2/decode_s3.png)
4. Lưu giá trị đã giải mã để sử dụng sau - bạn sẽ tận dụng giá trị đó trong phần tiếp theo để tạo quy tắc chặn

##### Chặn Bài kiểm tra bí ẩn

Sử dụng những gì bạn đã học trong các bài tập trước, hãy xây dựng quy tắc WAF tùy chỉnh để chặn giá trị tiêu đề **MysteryHint** bằng câu lệnh thích hợp. Hãy nghĩ về:

- Bạn nên kiểm tra phần nào của yêu cầu?
- Bạn nên chỉ định điều kiện **Kiểu khớp** nào?
- Bạn nên sử dụng **Chuyển đổi văn bản** nào?
- Hãy thử tự tạo quy tắc WAF tùy chỉnh
- nếu bạn cần trợ giúp, hãy mở rộng phần bên dưới để biết gợi ý

**Chặn Bài kiểm tra bí ẩn**

Trong phần “Đặt mức độ ưu tiên của quy tắc”, hãy chọn Lưu.

![1.1](/images/4/2/block_s3.png)
##### Xác thực khối Mystery Test của bạn

Xem lại bảng điều khiển hội thảo và xác minh rằng **MysteryTest** và tất cả các bài kiểm tra tự động khác đều vượt qua. Tham khảo phần Đánh giá để biết hướng dẫn. Lưu ý rằng không có quét thủ công nào được xây dựng cho bài kiểm tra này.

![1.1](/images/4/2/e_s1.png)

![1.1](/images/4/2/e_s2.png)
> **Xin chúc mừng!** Bạn đã khám phá thành công dữ liệu nhật ký AWS WAF và chặn Mystery Test.