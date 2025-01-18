---
title : "Protect APIs with Request Body Parsing"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 3.6 </b> "
---

### Scenario

Your business has developed an API that allows partners to retrieve a product list, with a maximum limit of 100 products per request. To enhance security, a WAF protection layer needs to be added. Your task is to configure the protection to allow only valid JSON requests and ensure that the numrecords value is within the range of 1 to 100.

Valid JSON example for this API:
```bash
{ "numrecords":"25" }
```

Only requests with valid JSON syntax and a numrecords value between 1 and 100 will be accepted. The API is available at the path **/api/listproducts.php**. Requests that fail validation must be rejected with an HTTP 400 response code.


### Instructions

First, create a regex pattern to validate the value of numrecords (1-100):

```bash
^0*(?:[1-9][0-9]?|100)$
```

Then, create a WAF rule consisting of two statements:
1. Check the URI path of the request (must match /api/listproducts.php).
2. Verify that the JSON body has a valid syntax and that the numrecords value matches the regex pattern.


### Procedure


The procedure below is longer and more complex than the previous sections. Please pay close attention to the steps outlined below.

#### Create the Regex Pattern Set
Create a regex pattern set that matches numbers 1-100:

1. Navigate to the Regex pattern sets section in WAF.

2. Select Create regex pattern set.

3. Regex pattern set name: number-1-to-100

4. Regular expressions: **^0*(?:[1-9][0-9]?|100)$**

5. Click **Create regex pattern**

![1.1](/images/3/6/regrex.png)
#### Create the WAF Rule

1. In the Web ACL, navigate to the Rules tab.

2. Select Add Rule.

3. Select Add my own rules and rule groups.

![1.1](/images/3/6/add_rule.png)
#### Rule details

1. Rule name: api-protection
2. Type: Regular rule
3. If a request: Matches all the statements (AND) 

![1.1](/images/3/6/rule_details.png)
#### Statement 1

1. Inspect: URI path
2. Match type: Exactly matches string
3. String to match: /api/listproducts.php
4. Text transformation: none 

![1.1](/images/3/6/s1.png)
#### Statement 2

1. Check Negate statement results
2. Inspect: Body
3. Content type: JSON
4. JSON match scope: Values
5. How AWS WAF should handle the request if the JSON in the request body is invalid: **No match** 

![1.1](/images/3/6/s2.png)

Verify JSON element matches regex statement:

1. Content to inspect: Only included elements
2. Included elements: /numrecords
3. Match type: Matches pattern from regex pattern set
4. Regex pattern set: number-1-to-100
5. Text transformation: None
6. Oversize handling: Continue 
![1.1](/images/3/6/s2b.png)
#### Match action

1. Action: Block
2. Custom response: Enable
3. Response Code: 400
4. Click Add rule 

![1.1](/images/3/6/then.png)
#### Set rule priority

1. Click Save on the Set rule priority page.

![1.1](/images/3/6/prio_s1.png)
2. On the Rules tab, verify that the new custom rule has been added successfully and listed.

#### Assess the effectiveness of the protection



1. Perform a manual scan from the Evaluate section and ensure that the API Misuse test passes successfully.

![1.1](/images/3/6/e_s1.png)

2. Review the workshop dashboard to verify that the API Misuse automated test has passed.

![1.1](/images/3/6/e_s2.png)