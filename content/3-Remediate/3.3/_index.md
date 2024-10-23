---
title : "Gain visibility into Bot Traffic"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

### Scenario
Your organization's security team suspects that a large fraction of traffic on your website comes from various bots, both wanted (such as search engines) and unwanted bots (such as content scrapers). You need to get better data on the number and type of bots on your website. You should NOT block any bot traffic yet, at least not until you have had a chance to analyze all sources of traffic. Your organization also wants to scope the use of bot control for cost control and avoid applying unneeded protections on static objects (CSS stylesheets, JS files, etc.). Web developers have provided the following RegEx statement that matches all of the static content on the website:

```(?i)\.(jpe?g|gif|png|svg|ico|css|js|woff2?)$```

### Instructions
You can use the AWS WAF Bot Control managed rule set to gain visibility into the bot traffic on your site. The Bot Control managed rule group provides options for common and targeted bots. The common bots option adds labels to self-identifying bots, verifies generally desirable bots, and detects high confidence bot signatures, targeted bots focuses on pervasive bots. For more details, please review [documentation](https://docs.aws.amazon.com/waf/latest/developerguide/waf-bot-control.html).

Create a **regex pattern** set that matches the static content. Setup the AWS WAF Bot Control rule group, use **scope-down** to exclude the static content (using the regex pattern), and ensure it's only counting the traffic (override the default behavior). Note that the AWS WAF Bot Control rule group will still apply AWS WAF labels to bot requests. You will be able to use those labels to control requests on a granular level.

After several minutes, you can navigate to the Bot Control tab to view additional details about bot traffic on your website.

{{% notice info %}}
This task does not have any protection validation steps. You will use labels applied by the AWS WAF Bot Control rule group in the next two tasks.
{{% /notice %}}


### Procedure
#### Regex Pattern Set
First, create a regex pattern set to filter out static content based on developer input.

1. Navigate to Regex pattern sets and verify WAF console region selection is correct

![1.1](/images/3/3/regrex_s1.png)
2. There is a regex pattern already set up for you. Click it for more detail

![1.1](/images/3/3/regrex_s3.png)
Regular expressions: *(?i)\.(jpe?g|gif|png|svg|ico|css|js|woff2?)$*


### AWS WAF Bot Control rule set
Next, enable AWS WAF Bot Control to gain visibility into bot traffic. Override rule actions to count to make sure you are not blocking any bot traffic. This will count the number of matches without blocking any traffic. Use scopedown statements to avoid scanning static objects that match the regex pattern.

#### Start adding AWS WAF Bot Control
1. Open the waf-workshop-webacl in the AWS WAF console and select the Traffic overview tab

![1.1](/images/3/3/start_s1.png)

2. In the Data filters section, select the Bot Control tab


3. In the How it works section, click on Add AWS WAF Bot Control rule group

![1.1](/images/3/3/regrex_s3.png)
#### Bot Control configuration
1. For Bot Control inspection level, choose Common as the inspection level

![1.1](/images/3/3/bot_s1.png)

2. In Bot Control Rules, set Override all rule actions to Count 

![1.1](/images/3/3/bot_s2.png)
#### Scope-down statement
1. Choose **Only inspect requests that match a scope-down statement**

2. Scope-down statement: enabled (checked)


3. If a request: doesn't match the statement (NOT)

4. Inspect: URI path


5. Match type: Matches pattern from regex patttern set

6. Regex pattern set: static-content (the regex pattern set created above)

7. Text transformation: None 

![1.1](/images/3/3/scope_s1.png)

### Set priority and review rules
1. On the Set rule priority page, click on Save (no changes needed)

![1.1](/images/3/3/prio_s1.png)

1. Verify that Bot Control rule set is now listed on Rules tab in the Web ACL

![1.1](/images/3/3/prio_s2.png)

3. Within a few minutes you will gain additional metrics on the Bot Control tab
![1.1](/images/3/3/prio_s3.png)
4. Proceed to the next task to take advantage of the added visibility provided by AWS WAF Bot Control

Congratulations! You have successfully integrated AWS WAF Bot Control which will start applying labels to bot requests. You will use those labels in the following tasks.