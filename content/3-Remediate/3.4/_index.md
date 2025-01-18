---
title : "Block Bad Bots using Custom Rules"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

### Scenario

In the last section, AWS WAF Bot Control was enabled to monitor bot traffic. After analysis, you discovered that the bot "zyborg" is overloading your web server and is not allowed to operate within your application. Your task is to block all requests from the "zyborg" bot.

### Instructions

The AWS WAF Bot Control rule group will label requests from the "zyborg" bot with the tag:
**awswaf:managed:aws:bot-control:bot:name:zyborg**. Create a custom WAF rule to block all requests containing this label.

### Procedure
#### Add a custom rule to the Web ACL
Open Web ACL and navigate to the rule list. Click Add rules and select Add my own rules and rule groups.

![1.1](/images/3/4/navigate.png)
##### Rule details

1. Rule type: **Rule builder**
2. Name: **zyborg-block**
3. Type: **Regular rule**
4. If a request: **matches the statement**

![1.1](/images/3/4/rule_detail.png)
##### Statement

1. Inspect: **Has a label**
2. Match scope: **Label**
3. Match key: **awswaf:managed:aws:bot-control:bot:name:zyborg**


![1.1](/images/3/4/statement.png)
##### Match action

1. Action: **Block**

2. Click **Add rules** at the bottom of the page. 

![1.1](/images/3/4/then_1.png)
##### Set rule priority

1. On the **Set rule priority** page, ensure that the zyborg-block rule is placed below the AWS WAF Bot managed rule ("AWS-AWSManagedRulesBotControlRuleSet"). Then click **Save**.

![1.1](/images/3/4/prio_1.png)

 Rules tab and verify that the zyborg-block rule is listed.

2. Navigate to the **Rules** tab, verify that the new custom rule is now listed.

![1.1](/images/3/4/prio_2.png)

#### Assess the effectiveness of the protection

1. Use manual scanning tool in the **Evaluate** section to verify that testing with the "zyborg" bot returns a 403 Forbidden error.
![1.1](/images/3/4/e_s1.png)
2. Review the workshop console and ensure that automated scans with "Bad Bot" show a passed status (green).
![1.1](/images/3/4/e_s2.png)

Congratulations! You have successfully configured a rule to block the "zyborg" bot.