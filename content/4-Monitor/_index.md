---
title : "Monitor"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

In this section, you will learn how to monitor AWS WAF with CloudWatch and how to investigate its logs. AWS WAF records CloudWatch metrics for each WebACL and each WAF rule within the WebACL. In addition, AWS WAF in this workshop is preconfigured for real-time logging through Amazon Kinesis Firehose to Amazon S3. You will get a chance to investigate those logs by querying them through Amazon Athena.

You can check the configuration in the tag “Logging and metrics” tab in the Web ACLs page
![1.1](/images/4/1.png)

![1.1](/images/4/2.png)

You can also check the data schema in [Glue](https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#/v2/data-catalog/tables)

![1.1](/images/4/3.png)

The table schema

![1.1](/images/4/4.png)
HTTP request struct structure:

```bash 
{
  "httprequest": {
    "clientip": "string",
    "country": "string",
    "headers": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "uri": "string",
    "args": "string",
    "httpversion": "string",
    "httpmethod": "string",
    "requestid": "string"
  }
}
```
### Content
1. [Investigate AWS WAF Logs](4.1/)
2. [Block Mystery Test](4.2/)
3. [Insert Custom HTTP Request Header](4.3/)