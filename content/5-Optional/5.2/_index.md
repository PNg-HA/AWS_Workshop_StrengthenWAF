---
title : "Review the WAF Bot Dashboard"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Scenario

If you have enabled AWS WAF Bot detection rule in an earlier exercise, your Bot Control dashboard should contain interesting data to analyze. Review the dashboard to analyze the bot activity on your site.

#### Instructions

In your WebACL, navigate to the Bot Control tab and review the data for the last 1 hour. Use the dashboard to get a quick insight into bot traffic on your site:
- What percentage of traffic is bots?
- How many bots were allowed through in the last 5 minutes? How many were blocked?
- What is the top category of bots?
- How many times did **Zyborg** bot attempt to reach your website in the last 5 minutes?

> There are no validation or verification steps in this exercise.

#### Procedure
##### Navigate to the Bot Control dashboard

1. Navigate to the WebACL in the AWS WAF console

2. Select the **Bot Control** tab

![bot_dashboard](/images/5/2/bot_dashboard.png)

3. You should see graphs similar to the screenshot
![bot_dashboard](/images/5/2/bot-dashboard_2.png)

##### Collect the metrics

1. Check the data under **All requests, in percentages**

2. The screenshot below shows the section with a red rectangle

3. Click on **1h** in the top right of the dashboard

![1.1](/images/5/2/s2.png)

4. The chart will show data in 5-minute time intervals

5. Review the number of requests allowed vs. blocked in within the last 5-minute interval

![1.1](/images/5/2/s5.png)

6. The next graph down shows the top categories of bots

![1.1](/images/5/2/s6.png)
##### Collect Data on a Specific Bot

1. In **Browse bot activity** dropdown, select **Zyborg.**
2. The chart below will show data for the Zyborg bot (similar to this screenshot):

![1.1](/images/5/2/collect_data_s2.png)
> **Congratulations!** You have successfully learned how to quickly analyze bot traffic in the **Bot Control** dashboard.