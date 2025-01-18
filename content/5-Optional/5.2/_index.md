---
title : "Review the WAF Bot Dashboard"
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Scenario

Have you enabled the AWS WAF Bot detection rule in an earlier exercise, your Bot Control dashboard should contain some interesting data for analyzation. Review the dashboard to analyze the bot activity on your site.

#### Instructions

Access your WebACL console. From there, switch to the Bot Control tab and review the recorded data from the last 1 hour. Use the dashboard to get a quick insight into bot traffic on your site, then answer the following questions.

- How much of the traffic is bots (in percentage)?
- How many bots were allowed and blocked through in the last 5 minutes?
- What is the top category of bots?
- How many times did **Zyborg** bot attempt to reach your website in the last 5 minutes?

> There are no validation or verification steps in this exercise.

#### Procedure
##### Navigate to the Bot Control dashboard

1. Navigate to the WebACL in the AWS WAF console

2. Switch to the **Bot Control** tab

![bot_dashboard](/images/5/2/bot_dashboard.png)

3. You should see graphs similar to those in the screenshot

![bot_dashboard](/images/5/2/bot-dashboard_2.png)

##### Collect the metrics

1. Check the data under **All requests, in percentages**

2. Click on **1h** in the top right of the dashboard

![1.1](/images/5/2/s2.png)

3. The chart will show the data of the last hour in 5-minute time intervals

4. Review the number of allowed requests in comparision with the block reports within the last 5 minutes.

![1.1](/images/5/2/s5.png)

5. The next graph down shows the top categories of bots

![1.1](/images/5/2/s6.png)
##### Collect Data on a Specific Bot

1. In **Browse bot activity** dropdown, select **Zyborg.**
2. The chart below will show data for the Zyborg bot (similar to this screenshot):

![1.1](/images/5/2/collect_data_s2.png)
> **Congratulations!** You have successfully learned how to quickly analyze bot traffic in the **Bot Control** dashboard.