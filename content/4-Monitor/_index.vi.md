---
title : "Monitor"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

Trong phần này, bạn sẽ học cách giám sát AWS WAF bằng CloudWatch và cách điều tra nhật ký của nó. AWS WAF ghi lại số liệu CloudWatch cho từng WebACL và từng quy tắc WAF trong WebACL. Ngoài ra, AWS WAF trong hội thảo này được cấu hình sẵn để ghi nhật ký theo thời gian thực thông qua Amazon Kinesis Firehose tới Amazon S3. Bạn sẽ có cơ hội điều tra các nhật ký đó bằng cách truy vấn chúng thông qua Amazon Athena.

Bạn có thể kiểm tra cấu hình trong thẻ “Ghi nhật ký và số liệu” trong trang Web ACL
![1.1](/images/4/1.png)

![1.1](/images/4/2.png)

Bạn cũng có thể kiểm tra lược đồ dữ liệu trong [Glue](https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#/v2/data-catalog/tables)

![1.1](/images/4/3.png)

Lược đồ bảng

![1.1](/images/4/4.png)
Cấu trúc cấu trúc yêu cầu HTTP:

```bash
{
"httprequest": {
"clientip": "string",
"country": "string",
"headers": [
{
"name": "string",
"value": "string"
}
],
"uri": "string",
"args": "string",
"httpversion": "string",
"httpmethod": "string",
"requestid": "string"
}
}
```
### Nội dung
1. [Điều tra Nhật ký AWS WAF](4.1/)
2. [Kiểm tra bí ẩn khối](4.2/)
3. [Chèn Tiêu đề yêu cầu HTTP tùy chỉnh](4.3/)