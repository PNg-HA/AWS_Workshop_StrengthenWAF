---
title : "Giới thiệu"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

### Giới thiệu AWS WAF
**AWS WAF(AWS Web Application Firewall)** là dịch vụ tường lửa ứng dụng web. Dịch vụ này giúp bảo vệ các ứng dụng web hoặc API của bạn khỏi các khai thác web phổ biến có thể ảnh hưởng đến tính khả dụng, xâm phạm bảo mật hoặc tiêu tốn quá nhiều tài nguyên.

Sử dụng WAF là một cách tuyệt vời để tăng cường khả năng phòng thủ sâu cho ứng dụng web của bạn. WAF có thể giúp giảm thiểu rủi ro của các lỗ hổng như [SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection), [Cross Site Scripting](https://owasp.org/www-community/attacks/xss/) và các cuộc tấn công phổ biến khác (được liệt kê trong Top 10 OWASP). WAF cho phép bạn tạo các quy tắc tùy chỉnh của riêng mình để quyết định xem có chặn hay cho phép các yêu cầu HTTP trước khi chúng đến ứng dụng của bạn hay không.

![làm thế nào](/images/1/how_waf_works.png)