---
title : "Protect a Web Form Using CAPTCHA"
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Scenario

The sample website (deployed as part of this workshop) contains a Quiz. You are required to install a AWS WAF CAPTCHA protection for the Quiz page to verify that only answers from legitimate users can pass through.

#### Instructions

Use the [AWS WAF CAPTCHA](https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge-best-practices.html) to ensure that the form hosted at /quiz can only receive access from humans. You will anually verify that the rule is working. After that, you'll solve the presented CAPTCHA puzzle, then solve the AWS WAF Quiz.

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
2. On the **Rules** tab, verify that the new custom rule is now listed (it may be on the 2nd rules page)

![1.1](/images/5/3/prio_s2.png)

3. You have successfully added the custom rule with CAPTCHA that only allows access from humans. You are now ready to give your newly form protection some test.

##### Evaluate protection effectiveness

1. Navigate to the Sample web application.

2. Click the link in the top right corner to open the **AWS WAF Quiz** .

![1.1](/images/5/3/e_s2.png)

3. You will come across a page asking you to confirm you are human. Solve the puzzle on the page.

![1.1](/images/5/3/e_s3.png)

4. You will be briefly shown a **Success** message, then redirected to the Quiz page.

5. Solve the Quiz and click on the **Submit** button. You will be shown a pop-up message with the Quiz result.

![1.1](/images/5/3/e_s5.png)

> **Congratulations!** You have deployed **AWS WAF CAPTCHA** for a web form defense.