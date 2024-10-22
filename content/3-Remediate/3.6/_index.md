---
title : "Protect APIs with Request Body Parsing"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 3.6 </b> "
---

### Scenario
The web store has built an API to help with integrations with their partners. This API is available to retrieve a list of up to 100 products at a time. The development team is working on adding layers of security in the API code. Your task is to add WAF protection that will only allow valid JSON requests and limit the number of listed products to a number between 1 and 100.

Here is a sample valid JSON used to invoke this API:
```bash
{ "numrecords":"25" }
```
Only requests with the valid JSON formatting and the valid value for numrecords (1-100 inclusive) should be allowed. The API is available at the path **/api/listproducts.php**. For API requests that do not pass all of the validation steps, the WAF rule should return a 400 http response code. According to RFC 2616, the 400 http response code indicates that the request could not be understood and the client should not repeat the request without modifications.

### Instructions
First, create a regex pattern set that matches the valid value (1-100) for numrecords. Here is a regular expression that you can use:
```bash
^0*(?:[1-9][0-9]?|100)$
```
Then, create a WAF rule with two statements. One statement should match the URI path of the API (/api/listproducts.php). Another statement needs to inspect the JSON body of incoming API requests, and validate that the JSON syntax is valid. It should also validate the value of "numrecords" using the regex pattern set. If the URI path matches, but the syntax is not correct, then the WAF rule should return a custom response using a 400 http response code.

After implementing the new WAF rule to guard against API abuse, test your protection using both manual and automated scans. For manual scans, click on the link in the event outputs. For automated scans, check the Progress Dashboard in Event Outputs .

### Procedure
The procedure below is lengthier and more complex than in previous tasks. Please pay special attention to the steps listed below.

#### Regex Pattern Set
Create a regex pattern set that matches numbers 1-100:

1. Navigate to Regex pattern sets and verify the WAF console region selection is correct

2. Click on Create regex pattern set 

3. Regex pattern set name: number-1-to-100

4. Regular expressions: **^0*(?:[1-9][0-9]?|100)$**

5. Click on **Create regex pattern**

![1.1](/images/3/6/regrex.png)
#### WAF Rule

1. Create a regular WAF rule that matches all the statements:

2. Navigate to the Rules tab of the Web ACL

3. Click on Add Rules and select Add my own rules and rule groups 

![1.1](/images/3/6/add_rule.png)
#### Rule details

1. Rule name: api-protection
2. Type: Regular rule
3. If a request: Matches all the statements (AND) 

![1.1](/images/3/6/rule_details.png)
#### Statement #1

1. Inspect: URI path
2. Match type: Exactly matches string
3. String to match: /api/listproducts.php
4. Text transformation: none 

![1.1](/images/3/6/s1.png)
#### Statement #2

1. Negate statement results (checked)
2. Inspect: Body
3. Content type: JSON
4. JSON match scope: Values
5. How AWS WAF should handle the request if the JSON in the request body is invalid: **No match** 

![1.1](/images/3/6/s2.png)

Match the specific JSON element with the regex statement:

1. Content to inspect: Only included elements
2. Included elements: /numrecords
3. Match type: Matches pattern from regex pattern set
4. Regex pattern set: (previosly created pattern set matching numbers 1-100)
5. Text transformation: None
6. Oversize handling: Continue 
![1.1](/images/3/6/s2b.png)
#### Then

1. Action: Block
2. Custom response: Enable (checked)
3. Response Code: 400
4. Click on Add rule 

![1.1](/images/3/6/then.png)
#### Rule priority

1. On the Set rule priority page, click on Save 

![1.1](/images/3/6/prio_s1.png)
2. On the Rules tab, verify that the new custom rule is now listed 

3. You have completed adding the custom rule that checks JSON values in the body of the request and protects your API against incorrect usage. You are now ready to test the protection.

#### Evaluate protection effectiveness
Use both manual and automated testing methods to evaluate the effectiveness of the new rule in your ACL:

1. Use manual scanning from the Evaluate section. Validate that API Misuse test is passing.

![1.1](/images/3/6/e_s1.png)
2. Review the workshop dashboard and verify that the API Misuse automated scan is passing (the test should be color-coded green). Refer to the Evaluate section for instructions.

![1.1](/images/3/6/e_s2.png)