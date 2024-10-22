---
title : "Protect a Web Form Using CAPTCHA"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Scenario

The sample website deployed as part of this workshop contains a Quiz. Please add AWS WAF CAPTCHA protection to the Quiz page to ensure that answers are coming from legitimate users.

#### Instructions

Use the [AWS WAF CAPTCHA](https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge-best-practices.html) to ensure that only humans can access the form hosted at /quiz. Validate manually that the rule is working. Solve the presented CAPTCHA puzzle, and solve the AWS WAF Quiz.

#### Procedure
##### Add a custom rule to the Web ACL:

1. Navigate to the **Rules** tab of the Web ACL

2. Click on **Add Rules** and select **Add my own rules and rule groups**

![1.1](/images/5/3/s1.png)
**Rule details**

1. Rule type: **Rule builder**
2. Name: **quiz-captcha**
3. Type: **Regular rule**

![1.1](/images/5/3/rule_detail.png)

**Statement**

1. If a request: **Matches the statement**
2. Inspect: **URI path**
3. Match type: **Exactly matches string**
4. String to match: **/quiz**
5. Text transformation: **None**

![1.1](/images/5/3/statement.png)

**Then**

1. Action: **CAPTCHA**
2. Check **Set a custom immunity time for this rule**
3. Immunity time: **900** seconds
4. Click on **Add rules** on the bottom of the page

![1.1](/images/5/3/then.png)

**Rule priority**

1. On the **Set rule priority** page, click on **Save**
2. On the **Rules** tab, verify that the new custom rule is now listed (it may be on the 2nd page of rules)

![1.1](/images/5/3/prio_s2.png)

3. You have completed adding the custom rule with CAPTCHA that allows access only to humans. You are now ready to test the protection.

##### Evaluate protection effectiveness

1. Navigate to the Sample web application.

2. Open the **AWS WAF Quiz** using the link in the top right corner.

![1.1](/images/5/3/e_s2.png)

3. You will be then shown a page asking you to confirm you are human. Solve the puzzle that is shown to you.

![1.1](/images/5/3/e_s3.png)

4. You will be briefly shown a **Success** message, and you will be redirected to the Quiz page.

5. Solve the Quiz and click on the **Submit** button. You will be shown a pop-up message with the Quiz result.

![1.1](/images/5/3/e_s5.png)

> **Congratulations!** You have successfully implemented **AWS WAF CAPTCHA** protection for a web form.