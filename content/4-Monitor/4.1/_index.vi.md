---
title : "Điều tra Nhật ký AWS WAF"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1. </b> "
---

#### Kịch bản

Trước đây chúng ta đã khám phá cách AWS WAF cung cấp số liệu CloudWatch để giám sát và thiết lập cảnh báo, nhưng để khắc phục sự cố cụ thể, chúng ta thường cần vượt ra ngoài các số liệu đó và kiểm tra dữ liệu thô. Nhật ký AWS WAF cung cấp thông tin chi tiết về mọi yêu cầu được xử lý bởi Web ACL của bạn. Phần này sẽ hướng dẫn bạn cách tận dụng các nhật ký này để điều tra chuyên sâu.

#### Hướng dẫn

Sử dụng Amazon Athena để khám phá nhật ký AWS WAF. Lưu ý rằng nhật ký AWS WAF đã được định cấu hình cho bạn và bảng **waf_access_logs** của Amazon Athena trỏ đến các nhật ký nằm trong S3. Nhiệm vụ đầu tiên là xác định các tiêu đề HTTP phổ biến nhất trong các yêu cầu. Tiếp theo, tìm quy tắc nào đang chặn nhiều yêu cầu nhất và trên đường dẫn URI nào. Xác định các quy tắc đang chặn quyền truy cập vào API nằm tại **/api/listproducts.php**. Cuối cùng, hãy tìm ra đường dẫn URI nào bị chặn bởi **Bộ quy tắc cốt lõi** và bởi các quy tắc WAF tùy chỉnh chặn đường dẫn mà bạn đã tạo trước đó.

> Không có bước xác minh nào trong bài tập này.

#### Quy trình
##### Điều tra Nhật ký AWS WAF bằng Truy vấn Amazon Athena
> Hội thảo này được cấu hình sẵn với cơ sở dữ liệu AWS Glue, nhóm làm việc Amazon Athena và truy vấn Athena. Phần này sẽ hướng dẫn bạn cách chuyển sang nhóm làm việc Amazon Athena chính xác và truy cập các truy vấn.

1. Mở **Amazon Athena** trong AWS Console: https://console.aws.amazon.com/athena

2. Trong danh sách thả xuống nhóm làm việc, hãy chọn tên nhóm làm việc **waf-workshop**

![1.1](/images/4/1/2.png)
3. Nếu hiển thị cửa sổ bật lên cài đặt, hãy nhấp vào **Xác nhận**
![1.1](/images/4/1/3.png)
4. Đi đến tab **Truy vấn đã lưu**

5. Nhấp vào hàng để mở Truy vấn đã lưu có tên là **MostPopularHttpHeaders**
![1.1](/images/4/1/5.png)
6. Chạy truy vấn
![1.1](/images/4/1/6a.png)

Sau khi chạy truy vấn và không thành công, nếu bạn gặp lỗi” TABLE_NOT_FOUND: dòng 1:37: Bảng 'awsdatacatalog.default.waf_access_logs' không tồn tại”.
![1.1](/images/4/1/6b.png)

Đảm bảo tên cơ sở dữ liệu là glueaccesslogsdatabase-* có bảng có tên là “waf_access_logs”
![1.1](/images/4/1/6c.png)

> Truy vấn này giúp chúng tôi hiểu [Tiêu đề HTTP](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields) nào đang được truyền và tần suất sử dụng từng tiêu đề. Mong đợi sẽ thấy các giá trị khác nhau trong kết quả của bạn, vì chúng phụ thuộc vào số lượng yêu cầu đã được gửi và ghi lại.

![1.1](/images/4/1/6d.png)
##### Điều tra các quy tắc chặn

Một trong những nhiệm vụ chung của quản trị viên WAF là hiểu tác động của các quy tắc WAF đối với các phần cụ thể của trang web của bạn. Bây giờ bạn sẽ nhập truy vấn tùy chỉnh của riêng mình để xác định quy tắc WAF nào đang chặn yêu cầu và đến đường dẫn URI cụ thể nào.

1. Nhấp vào biểu tượng + để bắt đầu truy vấn Amazon Athena mới

2. Sao chép truy vấn sau và dán vào cửa sổ truy vấn mới:

```bash
SELECT COUNT(*) AS count, terminatingruleid, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE action='BLOCK'
GROUP BY terminatingruleid, httprequest.clientip, httprequest.uri
ORDER BY count DESC
LIMIT 100
```

3. Sửa đổi truy vấn trên để tìm ra quy tắc nào đang chặn yêu cầu đến API nằm tại /api/listproducts.php. Trước tiên, hãy thử tự sửa đổi truy vấn SQL và nếu bạn cần trợ giúp, hãy mở rộng phần bên dưới để biết gợi ý truy vấn.

**Gợi ý truy vấn**

```
SELECT COUNT(*) AS count, terminatingruleid, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE action='BLOCK' AND httprequest.uri = '/api/listproducts.php'
GROUP BY terminatingruleid, httprequest.clientip, httprequest.uri
ORDER BY count DESC
LIMIT 100
```
![1.1](/images/4/1/f1.png)
##### Tìm tần suất và quy tắc chặn quyền truy cập

1. Chạy truy vấn sau để xác định các yêu cầu bị chặn bởi Bộ quy tắc chung

```
SELECT COUNT(*) AS count, action, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE terminatingruleid='AWS-AWSManagedRulesCommonRuleSet'
GROUP BY action, httprequest.clientip, httprequest.uri
ORDER BY count DESC
LIMIT 100
```
![1.1](/images/4/1/often_s1.png)

2. Sửa đổi truy vấn này để tìm ra những yêu cầu nào đã bị chặn bởi quy tắc chặn đường dẫn bảo vệ thư mục /includes (quy tắc tùy chỉnh mà bạn đã tạo trong giai đoạn khắc phục). Hãy thử tự sửa đổi truy vấn SQL và nếu bạn cần trợ giúp, hãy mở rộng phần bên dưới để biết gợi ý truy vấn.

**Gợi ý truy vấn**

```
SELECT COUNT(*) AS count, terminatingruleid, action, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE action='BLOCK' AND httprequest.uri LIKE '/includes/%'
GROUP BY action, httprequest.clientip, httprequest.uri, terminatingruleid
ORDER BY count DESC
LIMIT 100
```
![1.1](/images/4/1/often_s2.png)

> **Xin chúc mừng!** Bạn đã khám phá thành công dữ liệu nhật ký AWS WAF để biết thêm chi tiết về các yêu cầu bị chặn.