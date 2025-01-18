---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Introduce AWS WAF

**AWS Web Application Firewall (WAF)**, which is a web application firewall AWS service, provides the defense for web applications and APIs to secure from well-known web attacks that may affect availability and consume excessive resources.

A web application is secured in-depth with web application firewall. Specifically, a WAF can prevent attackers exploit common vulnerabilities in Top 10 OWASP, such as [SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection), [Cross Site Scripting](https://owasp.org/www-community/attacks/xss/). Moreover, you can create your custom rules in WAF to filter or block inbound or outbound HTTP(S) traffic.

![how](/images/1/how_waf_works.png)