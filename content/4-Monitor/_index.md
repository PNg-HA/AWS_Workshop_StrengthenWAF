---
title : "Monitor"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

This lab section will provide you with some basic hands-on exprerience with using CloudWatch to monitor AWS WAF and investigating its logs. CloudWatch metrics are used for each ACL and each WAF rule in it. Besides, this workshop has AWS WAF preconfigured for real-time logging to Amazon S3 via Amazon Kinesis Firehose. You will be guided to investigate those logs with Amazon Athena queries.

You can have a look at the configuration in the “Logging and metrics” tab in the AWS WAF > Web ACLs > \<ACL name\> page.

![1.1](/images/4/1.png)

![1.1](/images/4/2.png)

Data schema can be checked in [AWS Glue](https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#/v2/data-catalog/tables)

![1.1](/images/4/3.png)

The table schema

![1.1](/images/4/4.png)
HTTP request structure:

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