---
title : "Block Bad Bots with Custom Rules"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

### Scenario
In the previous activity, you have enabled AWS WAF Bot Control to collect data on bot activity. After analyzing the bot activity, you have determined that "zyborg" bots are causing excessive load on web servers, and are not allowed bots in your app. You need to block all requests from zyborg bots.

### Instructions
The AWS WAF Bot Control rule group adds this label to requests from zyborg bots: **awswaf:managed:aws:bot-control:bot:name:zyborg**. Create a WAF rule with one statement that blocks all requests with this label.

After implementing the new WAF rule to block zyborg bot requests, test your new rule using both manual and automated scans.

### Procedure
#### Add a custom rule to the Web ACL
Navigate to the Web ACL and its list of rules. Click on "Add rules", select "Add my own rules and rule groups".

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
##### Then

1. Action: **Block**

2. Click on **Add rules** on the bottom of the page 

![1.1](/images/3/4/then_1.png)
##### Rule priority

1. On the **Set rule priority** page, ensure that the new zyborg-block rule appears **BELOW** the AWS WAF Bot managed rule ("AWS-AWSManagedRulesBotControlRuleSet") and **Save** 

![1.1](/images/3/4/prio_1.png)

2. On the **Rules** tab, verify that the new custom rule is now listed 

![1.1](/images/3/4/prio_2.png)
3. You have completed adding the custom rule to block the zyborg bot. You are now ready to test the protection.

#### Evaluate protection effectiveness

1. Use manual scanning from the **Evaluate** section. Validate that Bad Bot test is passing and returning 403 Forbidden on that request.
![1.1](/images/3/4/e_s1.png)
2. Review the workshop dashboard and verify that Bad Bot automated scan is passing (green). Refer to the Evaluate section for instructions.
![1.1](/images/3/4/e_s2.png)

Congratulations! You have successfully protected your website against the zyborg bot.