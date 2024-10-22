---
title : "Rate Limit Bot Traffic and Send Custom Responses"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

### Scenario
Upon further analysis, the website management team identified one of your partners uses a PHP crawler bot and it occasionally creates spikes of traffic, slowing down the website for other users. The partner has been notified to work on reducing those spikes. While you await their fix, you need to rate-limit their automated traffic to avoid any further impact on the website performance. The rate limit should be 100 requests per 5 minutes. After the limit is reached, the response should follow RFC 6585 by returning http error code 429 (too many requests) with a **Retry-After** header indicating that requester should wait 900 seconds (15 minutes) before making a new request.

To test your rule, go to the Stack Outputs tab in CloudFormation page, and use the link called Trigger Rate Limiting. That will generate excessive requests against your site, simulating automated traffic. Once the rate-limiting rule is triggered, responses should include the correct HTTP response status code and the correct header.

### Instructions
Create a rate-limiting rule that allows up to 100 requests in 5 minutes, matching the label awswaf:managed:aws:bot-control:bot:name:phpcrawl. This rule should block with a custom response, response code **429**. Insert a custom header key **Retry-After** and value **900**. Create a custom response body that contains a message explaining the reason for the block: "Only 100 requests in a 5-minute period are allowed".

### Procedure

{{% notice info %}}
Verification steps for this rate-limiting differ from previous tasks. Please review the verification steps carefully.
{{% /notice %}}

#### Add a custom rule to the Web ACL:
1. Navigate to the Rules tab of the Web ACL

2. Click on Add Rules and select Add my own rules and rule groups

![1.1](/images/3/5/add_rule.png)
#### Rule details

1. Rule type: Rule builder
2. Name: phpcrawl-rate-limiter
3. Type: Rate-based rule 

![1.1](/images/3/5/rule_detail.png)
#### Request rate details

1. Rate limit: 100 in Evaluation window 5 minutes
2. Request aggregation: Source IP address
3. Scope of inspection and rate limiting: Only consider requests that match the criteria in a rule statement 

![1.1](/images/3/5/request.png)
#### Count only the requests that match the following statement

1. If a request: matches the statement
2. Inspect: Has a label
3. Match Key: awswaf:managed:aws:bot-control:bot:name:phpcrawl 

![1.1](/images/3/5/count.png)
#### Then

1. Action: Block

2. Custom response: Enable (checked)

3. Response Code: 429

4. Click on Add new custom header

5. Response Headers:
- Key: Retry-After
- Value: 900

6. Click on "Add rule" 

![1.1](/images/3/5/then.png)
#### Rule priority

1. On the Set rule priority page, ensure that the new phpcrawl-rate-limiter rule appears in the order BELOW the AWS WAF Bot Control managed rule (AWS-AWSManagedRulesBotControlRuleSet)

2. Click on Save 

3. On the Rules tab, verify that the new custom rule is now listed 

4. You have completed creating the custom rule to rate-limit PHP crawler requests. Please note the unique steps below to validate this rule.

#### Evaluate protection effectiveness
Use the following manual method to validate the rate-limiting rule:

1. Go to the Stack Outputs tab in CloudFormation, and use the link called Trigger Rate Limiting


2. The script will simulate an excessive number of requests to your test site while simulating the PHPCrawler bot

3. Initially, requests will result in 200 OK responses 

![1.1](/images/3/5/e_s3.png)

4. When the rate-limiting rule is triggered, requests will be blocked and result in 429 Too Many Requests responses

5. Confirm that the Retry-After header is included in responses 

![1.1](/images/3/5/e_s5.png)
Congratulations! You have successfully rate-limited the PHPCrawler bot and prevented excessive requests to your website.