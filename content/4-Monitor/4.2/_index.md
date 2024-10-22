---
title : "Block Mystery Test"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2. </b> "
---

#### Scenario

Some requests from the automated scanner included the `mystery-hint` header. Your task to find the value of that header, decode it, and block future requests with the same value.

#### Instructions

Use Amazon Athena to explore AWS WAF logs and find the value of the `mystery-hint` header in requests. The value is base64 encoded, so you need to decode the text to find the value. Build a custom WAF rule to block the mystery-hint header value with the appropriate statement (consider text transformation that may be required).

Validate your rule by reviewing the automated test dashboard that **MysteryTest** automated scan is passing. If you have completed remediate tasks, then completing this exercise will turn the last tile on the WAF Dashboard **My Progress** tab to **green!**

#### Procedure
##### Switch to Amazon Athena Workgroup

You can skip this section if you are already logged into Amazon Athena console and have switched to the **waf-workshop** workgroup.

**Switch to waf-workshop Athena workgroup**

1. Navigate to the [Amazon Athena Console](https://console.aws.amazon.com/athena)

2. In the workgroup dropdown, select the **waf-workshop** workgroup name

3. If presented with the settings popup, click on **Acknowledge**

##### Find the value of the Mystery Hint

Use an Amazon Athena query to parse AWS WAF logs and find the value of the `mystery-hint` header.

1. Open the **Saved queries** tab and click on the **MysteryHintHeader** query.

![1.1](/images/4/2/find_s1.png)

2. **Run the query** to retrieve the encoded value of the header. Note the [Base64](https://en.wikipedia.org/wiki/Base64) encoded value of the `mystery-hint` header

![1.1](/images/4/2/find_s2.png)
3. You can use any method to decode the value; if you need help, the steps in the next section show you how to use Amazon Athena's built in functions to perform decoding. In this step, use  https://www.base64decode.org/, receive the decoded form of the value

![1.1](/images/4/2/find_s3.png)
##### Decode the value of the Mystery Hint

> To decode the header value, the following query uses the from_base64() and from_utf8() Athena functions. You can read more about supported functions in the [Athena User Guide](https://docs.aws.amazon.com/athena/latest/ug/functions.html).

1. Click on the + symbol to start a new Amazon Athena query

2. Rewrite the previously used Saved query **MysteryHintHeader** by copying it over to a new Query with additional lookup of the decoded value of the `mystery-hint` header:

```
from_utf8(from_base64('<encoded value>'))
```

If you need help with the query, please find the hint below:

**Query Hint**

```
WITH DATASET AS (SELECT header FROM waf_access_logs CROSS JOIN UNNEST(httprequest.headers) AS t(header))
SELECT DISTINCT header.name, header.value as encoded_header_value, from_utf8(from_base64(header.value)) as decoded_header_value
FROM DATASET
WHERE LOWER(header.name)='mystery-hint'
```

3. Run the query to retrieve the header value


![1.1](/images/4/2/decode_s3.png)
4. Save the decoded value for later - you will leverage that in the next section to create a blocking rule

##### Block the Mystery Test

Using what you have learned in the previous exercises, build a custom WAF rule to block the **MysteryHint** header value with the appropriate statement. Think about:

- Which part of the request should you inspect?
- Which **Match type** condition should you specify?
- Which **Text transformation** should you use?
- Try to create the custom WAF rule on your own
- if you need help, expand the section below for a hint

**Blocking the Mystery Test**

In the “Set rule priority” section, choose Save.

![1.1](/images/4/2/block_s3.png)
##### Validate your Mystery Test block

Review the workshop dashboard and verify that **MysteryTest** and all other automated tests are passing. Refer to the Evaluate section for instructions. Note that there is no manual scan built for this test.

![1.1](/images/4/2/e_s1.png)

![1.1](/images/4/2/e_s2.png)
> **Congratulations!** You have successfully explored AWS WAF log data and blocked the Mystery Test.