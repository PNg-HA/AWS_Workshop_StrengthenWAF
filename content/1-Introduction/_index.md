---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Introduce AWS WAF
**AWS WAF(AWS Web Application Firewall)** is a web application firewall service. It helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources.

Using a WAF is a great way to add defense in depth to your web application. A WAF can help mitigate the risk of vulnerabilities such as [SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection), [Cross Site Scripting](https://owasp.org/www-community/attacks/xss/) and other common attacks (which listed in Top 10 OWASP). WAF allows you to create your own custom rules to decide whether to block or allow HTTP requests before they reach your application.

![how](/images/1/how_waf_works.png)