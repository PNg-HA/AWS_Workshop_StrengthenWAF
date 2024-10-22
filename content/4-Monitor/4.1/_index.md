---
title : "Investigate AWS WAF Logs"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1. </b> "
---


#### Scenario

We previously explored how AWS WAF provides CloudWatch metrics for monitoring and setting alerts, but for troubleshooting specific issues, we often need to go beyond those metrics and examine the raw data. AWS WAF logs provide detailed information about every request processed by your Web ACL. This section will guide you through how to leverage these logs for in-depth investigations.

#### Instructions

Use Amazon Athena to explore AWS WAF logs. Note that AWS WAF logging has been configured for you and Amazon Athena table **waf_access_logs** pointed to logs located in S3. First task is to determine what are the most common HTTP headers in the requests. Next, find which rules are blocking the most requests and on which URI paths. Identify rules that are blocking access to the API located at **/api/listproducts.php**. Finally, find out which URI paths are blocked by the **Core Rule Set** and by the path-blocking custom WAF rules you created earlier.

> There are no verification steps in this exercise.

#### Procedure
##### Investigate AWS WAF Logs with Amazon Athena Queries
> This workshop comes preconfigured with an AWS Glue database, Amazon Athena workgroup and Athena queries. This section will walk you through switching to the correct Amazon Athena workgroup and accessing the queries.

1. Open **Amazon Athena** in the AWS Console: https://console.aws.amazon.com/athena 

2. In the workgroup dropdown, select the **waf-workshop** workgroup name

![1.1](/images/4/1/2.png)
3. If presented with the settings popup, click on **Acknowledge**
![1.1](/images/4/1/3.png)
4. Go to the **Saved queries** tab

5. Click the row to open the Saved Query called **MostPopularHttpHeaders**
![1.1](/images/4/1/5.png)
6. Run the query
![1.1](/images/4/1/6a.png)

After running the query and fail, if you meet error” TABLE_NOT_FOUND: line 1:37: Table 'awsdatacatalog.default.waf_access_logs' does not exist”.
![1.1](/images/4/1/6b.png)

Make sure the database name is glueaccesslogsdatabase-* that has a table named “waf_access_logs”
![1.1](/images/4/1/6c.png)

> This query helps us understand what [HTTP Headers](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields) are being passed and how frequently each one is used. Expect to see different values in your results, as they depend on how many requests were sent and logged.

![1.1](/images/4/1/6d.png)
##### Investigate blocking rules

One of the common tasks for WAF administrators is to understand the impact that WAF rules have on specific parts of your site. You will now enter your own custom query to determine which WAF rules are blocking requests and to which specific URI paths.

1. Click on the + symbol to start a new Amazon Athena query

2. Copy the following query and paste it into the new query window:

```bash
SELECT COUNT(*) AS count, terminatingruleid, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE action='BLOCK'
GROUP BY terminatingruleid, httprequest.clientip, httprequest.uri
ORDER BY count DESC
LIMIT 100
```

3. Modify the query above to find out which rules are blocking requests to the API located at /api/listproducts.php. Try to modify the SQL query on your own first, and if you need help, expand the section below for a query hint.

**Query Hint**

```
SELECT COUNT(*) AS count, terminatingruleid, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE action='BLOCK' AND httprequest.uri = '/api/listproducts.php'
GROUP BY terminatingruleid, httprequest.clientip, httprequest.uri
ORDER BY count DESC
LIMIT 100
```
![1.1](/images/4/1/f1.png)
##### Find how often and what a rule blocked access

1. Run the following query to identify requests that were blocked by the Common Rule Set

```
SELECT COUNT(*) AS count, action, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE terminatingruleid='AWS-AWSManagedRulesCommonRuleSet'
GROUP BY action, httprequest.clientip, httprequest.uri
ORDER BY count DESC
LIMIT 100
```
![1.1](/images/4/1/often_s1.png)

2. Modify this query to find out which requests were blocked by the path-blocking rule protecting the /includes directory (the custom rule you created in the remediate phase). Try to modify the SQL query on your own, and if you need help, expand the section below for a query hint.

**Query Hint**

```
SELECT COUNT(*) AS count, terminatingruleid, action, httprequest.clientip, httprequest.uri
FROM waf_access_logs
WHERE action='BLOCK' AND httprequest.uri LIKE '/includes/%'
GROUP BY action, httprequest.clientip, httprequest.uri, terminatingruleid
ORDER BY count DESC
LIMIT 100
```
![1.1](/images/4/1/often_s2.png)

> **Congratulations!** You have successfully explored AWS WAF log data for more details on blocked requests.