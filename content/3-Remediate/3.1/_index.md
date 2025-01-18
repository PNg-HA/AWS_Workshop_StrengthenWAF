---
title : "Implement Basic Protection using AWS Managed Rules"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

### Scenario
Your security team has prioritized protection against common attacks such as Cross-Site Scripting (XSS), path traversal, and SQL injection as a high priority. Your task is to find ways to mitigate the risks of web application attacks using AWS WAF. Your business wants a fully managed solution that does not require manually creating rules.

### Instructions
Review the Web ACL configuration for the web application. Add the **Core rule set** and **SQL database** managed rule groups to it. The Core rule set includes rules that apply to most web applications, providing protection against a wide range of threats, from basic to critical. The SQL database rule group are designed to block requests related to SQL database exploitation (e.g., SQL Injection). For more information about the Core Rule Set and SQL Database Managed Rule Groups, refer to the [AWS WAF documentation](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-list.html).

After implementing the Core rule set and the SQL Database rule group, test protections with manual and automatic scans using the **WAF Dashboard**.

### Procedure
#### Add managed rules to the WAF ACL
Open the WAF ACL and navigate to the Rules tab:

1. Open the [AWS WAF Console](https://console.aws.amazon.com/wafv2/homev2/web-acls?region=global).

2. Ensure that the Global (CloudFront) region selected.

![2](/images/3/1/2.png)
3. Open the WebACL named waf-workshop-webacl.

4. Navigate to the Rule tab. 

Add the Core rule set and the managed rule group for SQL database:

1. Click Add Rules, then select Add managed rule groups.
![1.1](/images/3/1/1.png)

2. Expand the AWS managed rule groups section.

3. Enable the toggle for the Core rule et (turns blue). (changes color to blue)

![1.1](/images/3/1/4a.png)
4. Enable the toggle for the SQL database (turns blue).
![1.1](/images/3/1/4b.png)
5. Click Add rules at the bottom of the page.

6. On the Set rule priority page, click on Save. 

7. On the Rule tab, confirm that both the AWS Managed Common and SQLi Rule Sets have been successfully added.

8. You have added two managed rule sets to the WebACL. You can proceed to test the protections.


### Assess the effectiveness of the protection
Perform manual protection testing following the instructions in the Evaluate section to ensure that SQL Insert, Cross-site Scripting (XSS), and Path Traversal tests are handled properly.
![1.1](/images/3/1/e1.png)
Review the progress dashboard to confirm that automatic scans for the above tests have passed.
![1.1](/images/3/1/e2.png)
You have successfully implemented the Core rule set and the SQL database rule set, providing essential protection against web application attacks!