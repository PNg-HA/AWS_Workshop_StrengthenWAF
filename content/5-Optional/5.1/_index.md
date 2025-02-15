---
title : "Create a CloudWatch Alarm for an AWS WAF Metric"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Scenario

Your organization concerns are your partner's use of web crawlers and the crawlers staying under rate-limiting rules. You are asked to setup an alerting system to notify you if the PHP crawler usage exceeds an specified threshold.

#### Instructions

First, we are going to create an SNS topic with no subscriber (there's no need for setting up the email notification). Then, we'll create a CloudWatch alarm to track the phpcrawl-rate-limiter rule. The alarm is triggered if any request is consistently blocked for 1 minute.

Verify your configuration by triggering rate limiting. The script's excessive request should trigger the alarm (change the alarm state).

#### Procedure
##### Create an SNS topic
> **Note**: You may see an error message referencing **KMS** permissions. You can safely ignore it as we do not use at-rest encryption features of SNS in this lab.

1. Find and navigate to Simple Notification Service (SNS) in the AWS console

2. Under **Create topic**, enter the name **waf-alerts**, then click **Next step**

3. Under **Details**, fill the following information in the blanks:
- Type: **Standard**
- Name: **waf-alerts** (should be pre-populated) 

![1.1](/images/5/1/s3.png)

1. Scroll down and click on **Create topic**

##### Create a CloudWatch alarm

1. Navigate to **CloudWatch** service in the AWS Console

2. Navigate to **Alarms, All alarms**

3. Click on **Create alarm**

![1.1](/images/5/1/alarm_s3.png)

4. In the "Create alarm", click on **Select metric**

![1.1](/images/5/1/alarm_s4.png)
5. Navigate to **WAFV2, Region, Rule, WebACL**

![1.1](/images/5/1/alarm_s5a.png)
![1.1](/images/5/1/alarm_s5b.png)

6. Select this metric:
- Rule: **rate-limiter**
- Metric Name: **BlockedRequests**

1. Click on **Select metric**

![1.1](/images/5/1/select_metric.png)

2. In the next page, change **Period** down to **1 minute**

![1.1](/images/5/1/metric_s2.png)

3. Under "Conditions", change **than...** field to **0**, click *Next*

![1.1](/images/5/1/metric_s3.png)

4. Under **Notification**, click *Add Notification* and use the following values:
- Alarm state trigger: **In alarm**
- Select **Select an existing topic**
- Send a notification to...: **waf-alerts**

1. Accept default values, scroll down and click on **Next**

![1.1](/images/5/1/accept.png)

2. Under **Name and description** enter **crawler-alarm** as alarm name and click **Next**

![1.1](/images/5/1/accept_s2.png)

3. On the **Preview and create** page, click on **Create alarm**

![1.1](/images/5/1/accept_s3.png)

##### Verify the alarm configuration

This procedure will generate a large number of requests that will be recorded as Cloudwatch metrics, which will trigger the WAF rule for rate-limit. As the number of requests exceeds the limit, the alarm should be triggered

**Trigger the alarm**

1. Go to the Event Outputs and find a link called **Trigger Rate Limiting**
2. The script will simulate an excessive number of requests to your test site, simulating the automated traffic
3. Initially, requests will result in **200 OK** responses 

![1.1](/images/5/1/final_s3.png)

4. When the rate-limiting rule is triggerred, requests will be blocked. You will receive **429 Too Many Requests** responses instead.

![1.1](/images/5/1/final_s4.png)
**Verify in CloudWatch**

> It may take several minutes before the CloudWatch Alarms state changes into "In Alarm" from "Insufficient data".

1. Navigate to **CloudWatch** service in the AWS Console
2. Click **In alarm** to view triggerred alarms
3. You should see the alarm you just created (**Note:** it may take 1-2 minutes for the transition to in-alarm state)

4. Click **phpcrawl-alarm** to get more details with a chart


![1.1](/images/5/1/final.png)

> **Congratulations!** Your AWS WAF rule monitoring alarm has been successfully created. You'll surely be notified whenever the rule is triggered.