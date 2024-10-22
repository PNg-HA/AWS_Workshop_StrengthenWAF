---
title : "Apply Foundational Protection with AWS Managed Rules"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

### Scenario
Your security team has ranked protection against common attacks, such as Cross-Site Scripting (XSS), path traversal, and SQL injection (SQLi) as a high priority. Your task is to use AWS WAF to reduce the risk of these common web application attacks against your website. Your organization would prefer a fully managed solution that does not require manual creation of rules.

### Instructions
Review the Web ACL that is already configured for your web application. Add the **Core rule set** and **SQL database** managed rule groups to the WebACL. The Core rule set (CRS) rule group contains rules that are generally applicable to web applications. This provides protection against exploitation of a wide range of vulnerabilities, including high risk and commonly occurring vulnerabilities described in OWASP publications such as OWASP Top 10. The SQL database rule group contains rules to block request patterns associated with exploitation of SQL databases, like SQL injection attacks. Review the [AWS WAF documentation](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-list.html) for more details on rules included in the Core rule set and the SQL database rule group.

After implementing the Core rule set and the SQL Database rule group, test your protections using both manual and automated scans. Use the manual scanner, or check the **WAF Dashboard** for automated scans.

### Procedure
#### Add managed rules to the WAF ACL
Navigate to the WAF ACL and its Rules tab:

1. Open the [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/web-acls?region=global).

2. Verify that you are using the Global (CloudFront) region 

![2](/images/3/1/2.png)
1. Open the WebACL called waf-workshop-webacl 

2. Select the Rules tab 

Add the Core rule set and the SQL database managed rule groups:

1. Click on Add Rules and select Add managed rule groups 
![1.1](/images/3/1/1.png)

2. Expand AWS managed rule groups 

3. Click on the switch for the Core rule set (changes color to blue)

![1.1](/images/3/1/4a.png)
1. Click on the switch for the SQL database (changes color to blue) 
![1.1](/images/3/1/4b.png)
1. Click on Add rules on the bottom of the page


6. On the Set rule priority page, click on Save 

7. On the Rules tab, verify that AWS Managed Common and SQLi Rule Sets are both present 

8. You have added two managed rule sets to the WebACL. You are now ready to test them.


### Evaluate protection effectiveness
Follow the instructions to manually test protections from the Evaluate section to validate that SQL injection, Cross-site scripting (XSS) and Path traversal tests are passing.
![1.1](/images/3/1/e1.png)
Review the progress dashboard and verify that automated scans for tests listed above are passing.
![1.1](/images/3/1/e2.png)
Congratulations! You have successfully used Core rule set and SQL database rule set to apply foundational protections against web application attacks!