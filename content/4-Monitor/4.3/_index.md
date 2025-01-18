---
title : "Insert Custom HTTP Request Header"
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---

#### Scenario

The role of the Request Header Insertion feature is to assist the validation of requests made to the application checked by AWS WAF. The application can be configured to only allow requests containing the specified custom headers, or to have headers injected to process the requests in a different way, from the the header. It can also be configured to simply log the header in your application logs that may be used for reports and analytics.

> There are no verification steps in this exercise.

#### Instructions

Use the [request header insertion](https://docs.aws.amazon.com/waf/latest/developerguide/customizing-the-incoming-request.html) feature of AWS WAF. Verify the presence of the inserted headers with [Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html).

#### Procedure
##### Insert Custom Header:

1. In Web ACL, check the **Rules** tab 

2. Click on **Add Rules** and select **Add my own rules and rule groups**

![1.1](/images/4/3/s2.png)

Enter the following **Rule details**:

1. Rule type: **Rule builder**
2. Rule: **header-insert**, regular rule

![1.1](/images/4/3/detail.png)
**Statement**

1. If a request: **Matches the statement**
2. Inspect: **URI path**
3. Match type: **Starts with string**
4. String to match: **/api**

![1.1](/images/4/3/statement.png)

**Then**

1. Action: **Allow**
2. Custom request - optional: Key/Value: **request-source, TestBotCanary**
3. Click on **Add rule** on the bottom of the page

![1.1](/images/4/3/then.png)
**Rule priority**

1. On the **Set rule priority** page, click on **Save**
2. On the **Rules** tab, verify that the new custom rule is now listed
3. You have completed adding the custom rule that inserts the custom header.

![1.1](/images/4/3/prio.png)
> **Congratulations!** You have successfully configured custom header insertion.