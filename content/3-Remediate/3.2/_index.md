---
title : "Secure Paths using Custom Rules"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

### Scenario
Your website has a /includes directory containing files that should only be accessed by server processes. However, these files are currently accessible directly from the Internet, which poses a risk of exposing sensitive information. Your task is to configure AWS WAF to block all direct requests from the Internet to the /includes directory.

### Instructions
A custom AWS WAF rule can be used to block requests that have paths starting with /includes. To ensure that no encoded requests are missed, apply URL decoding transformations before checking. After configuring the custom rule and adding it to the Web ACL, verify the protection using both manual and automated scanning methods.

### Procedure
#### Add a custom rule to the Web ACL:
1. Open the Web ACL section and select the Rules tab.

2. Click Add Rule and choose Add my own rules and rule groups. 

![1.1](/images/3/2/2.png)
##### Rule details
1. Rule type: Rule builder
2. Name: path-block
3. Type: Regular rule
![1.1](/images/3/2/r1.png)

##### Statement

1. If a request: Matches the statement
2. Inspect: URI path
3. Match type: Starts with string
4. String to match: /includes
5. Text transformation: URL decode
![1.1](/images/3/2/s1.png)
##### Match action

1. Action: Block
2. Click Add rules at the bottom of the page.
![1.1](/images/3/2/t1.png)
##### Set rule priority

1. On the Set rule priority page, click Save.
![1.1](/images/3/2/p1_s1.png)
2. Return to the Rules tab and verify that the path-block rule is listed.
![1.1](/images/3/2/p1_s2.png)
3. You have now successfully added a custom rule to block all requests to the **/includes** directory. Next, test the protection.

#### Assess the effectiveness of the protection
Conduct manual testing to confirm that requests to the **/includes** directory result in a 403 Forbidden error.
![1.1](/images/3/2/e1.png)
Review the AWS WAF dashboard to confirm that automated scans verify requests to the **/includes** directory are blocked.

![1.1](/images/3/2/e2.png)
Congratulations! You have successfully protected the files in the **/includes** directory from external access.