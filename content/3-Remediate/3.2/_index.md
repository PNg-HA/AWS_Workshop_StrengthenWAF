---
title : "Protect Paths with Custom Rules"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

### Scenario
The website has a directory **/includes** with files that should only be accessed by processes on the server (for example, header and footer for server-side rendering). However, all files in that directory can be accessed directly from the Internet which can lead to unintended information disclosure. Your task is to block direct access to files in the /includes directory.

### Instructions
Create a custom AWS WAF rule that blocks all requests that begin with /includes. To prevent encoded requests from bypassing this rule, add a url decoding transformation before inspecting the request. After adding the custom rule to the Web ACL, test your protections using both manual and automated scans.

### Procedure
#### Add a custom rule to the Web ACL:
1. Navigate to the Rules tab of the Web ACL

2. Click on Add Rules and select Add my own rules and rule groups 

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
5. Text transformation: URL decode regular rule
![1.1](/images/3/2/s1.png)
##### Then

1. Action: Block
2. Click on Add rules on the bottom of the page regular rule
![1.1](/images/3/2/t1.png)
##### Rule priority

1. On the Set rule priority page, click on Save
![1.1](/images/3/2/p1_s1.png)
2. On the Rules tab, verify that the new custom rule is now listed regular rule
![1.1](/images/3/2/p1_s2.png)
3. You have completed adding the custom rule to block all requests to files in the /includes directory. You are now ready to test the protection.

#### Evaluate protection effectiveness
Follow the instructions to manually test protections from the Evaluate section to validate that Includes Modules test is passing and returning 403 Forbidden on that request.
![1.1](/images/3/2/e1.png)
Review the progress dashboard and verify that automated scans for tests listed above are passing. Request to URL that has path /includes has been blocked

![1.1](/images/3/2/e2.png)
Congratulations! You have successfully protected files in the **/includes** directory.