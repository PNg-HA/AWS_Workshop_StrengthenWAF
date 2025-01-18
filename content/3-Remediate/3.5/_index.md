---
title : "Control Bot Traffic Rates and Send Custom Responses"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

### Scenario
After analysis, your business has discovered that a partner is using a PHP crawler bot. This bot occasionally causes traffic spikes, impacting other users. While waiting for the partner to address the issue, you need to set up automatic traffic rate limits to ensure the website's performance.


### Instructions

Create a rate-limiting rule allowing a maximum of 100 requests within 5 minutes. The rule matches the label awswaf:managed:aws:bot-control:bot:name:phpcrawl. When the limit is exceeded, block the requests with a custom response containing:
- Response Code: **429**
- Response Header: key **Retry-After** and value **900**
- Response Body: "Only 100 requests are allowed within a 5-minute window."

### Procedure

{{% notice info %}}
Verification steps for this rate-limiting differ from previous tasks. Please review the verification steps carefully.
{{% /notice %}}

#### Add a custom rule to the Web ACL:
1. Open the Rules tab of the Web ACL.

2. Click Add rules and select Add my own rules and rule groups.

![1.1](/images/3/5/add_rule.png)
#### Rule details

1. Rule type: Rule builder
2. Name: phpcrawl-rate-limiter
3. Type: Rate-based rule 

![1.1](/images/3/5/rule_detail.png)
#### Configure request rate limiting:

1. Rate limit: 100 
2. Evaluation window: 5 minutes
3. Request aggregation: Source IP address
4. Scope of inspection and rate limiting: Only consider requests that match the criteria in a rule statement 

![1.1](/images/3/5/request.png)
#### Statement

1. If a request: matches the statement
2. Inspect: Has a label
3. Match Key: awswaf:managed:aws:bot-control:bot:name:phpcrawl 

![1.1](/images/3/5/count.png)
#### Match action

1. Action: Block
2. Custom response: Enable
3. Response Code: 429
4. Click Add new custom header
5. Response Headers:
- Key: Retry-After
- Value: 900
6. Click Add rule

![1.1](/images/3/5/then.png)
#### Set rule priority

1. On the Set rule priority page, place the phpcrawl-rate-limiter rule below the AWS WAF Bot Control managed rule.

2. Click Save.

3. Confirm the rule has been added to the list of rules on the Rules tab.

#### Assess the effectiveness of the protection

1. Navigate to Stack Outputs in CloudFormation and click the link Trigger Rate Limiting.

2. The link will simulate automated traffic by sending a large number of requests to the website.

3. Initially, requests receive 200 OK responses.

![1.1](/images/3/5/e_s3.png)

4. Once the limit is exceeded, responses switch to 429 Too Many Requests.

5. Confirm that the Retry-After header is included in the responses.

![1.1](/images/3/5/e_s5.png)
Congratulations! You have successfully set up a rate-limiting rule for the PHPCrawler bot, ensuring stable website performance.