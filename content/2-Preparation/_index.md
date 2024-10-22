---
title : "Preparation"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

### Download and execute the setup script

We will download file containing the workshop's resources, the CloudFormation template used to provision them, and the setup script. We will then run the script to deploy the workshop in your own AWS account.

1. Create a directory named "waf-workshop".

2. Run the following code (you can copy and paste all commands together):

```bash
# Download workshop assets
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/automated-scanner.zip'      --output automated-scanner.zip
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/manual-scanner.zip'         --output manual-scanner.zip
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/rate-limit-trigger.zip'     --output rate-limit-trigger.zip
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/sample-static-website.html' --output sample-static-website.html
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/assets/scanning-dashboard.html'    --output scanning-dashboard.html

# Download CloudFormation template and the setup script
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/static/waf-workshop.yaml' --output waf-workshop.yaml
curl 'https://static.us-east-1.prod.workshops.aws/public/9bf792a6-2354-4106-9e62-7e75544c4ccc/static/deploy-workshop-own-account.sh' --output deploy-workshop-own-account.sh
```
![1.1](/images/2/2.png)
3. Run the bootstrap script in the terminal
```bash
sh deploy-workshop-own-account.sh
```
If you meet the error *“syntax error: "(" unexpected”*, while running the script
![1.1](/images/2/3a.png)
Pleaze edit the first line of the script from “#!/bin/bash” to “#!/bin/bash” and save it.
![1.1](/images/2/3b.png)
and run the script with following command ```./deploy-workshop-own-account.sh```
![1.1](/images/2/3c.png)

If you receive the notification *“Failed to create/update the stack.”* then use the command ```aws cloudformation describe-stack-events --stack-name waf-workshop``` or check the Stack page in CloudFormation console to troubleshoot:
![1.1](/images/2/3d1.png)
![1.1](/images/2/3d2.png)
you will see that you have to adjust the MemorySize in the yaml file down to 3008.  
![1.1](/images/2/3d3.png)
After editting the yaml stack, run the script again. You will wait for 15 minutes then the task is deployed successfully.
![1.1](/images/2/3e.png)

Please remember to delete all deployed resources (CloudFormation stack) after completing the workshop to avoid unnecessary charges.

### Evaluate

This workshop will guide you through evaluating and improving the security posture of a web application. The environment you'll be working in consists of a web application hosted on AWS, intentionally designed to be vulnerable. Your objective is to identify these vulnerabilities and implement security measures to mitigate them.

Here's an overview of the steps in this section:
1. Understand the architecture: Understand the deployed architecture, and identify the underlying infrastructure components.

2. Access workshop progress dashboard: Explore the findings from automated scans of the web application, and your overall progress.

3. Run manual scans: Perform manual security scans on the web application and identify remaining challenges.

#### Workshop Architecture

The architecture of this workshop is shown as below:
![architecture](/images/waf-workshop-architecture.png)

Go to the Outputs tab of the Stack page of CloudFormation page.

![architecture](/images/2/output.png)

1. The first step is to find the website URL in the output named 1xWebApplicationURL. This sample website is already deployed in your AWS account for the workshop. Try opening it in a new tab. Your goal is to secure this website with AWS WAF.

2. You can access the AWS WAF console by using the link stored in the 2xWAFConsole output value. You will be using this console throughout the workshop, to apply protections to the web application you have just seen.

3. Open the link under 3xProgressDashboard in another tab. This dashboard tracks your progress throughout the workshop and updates automatically as you complete tasks.

4. You can also manually test your WAF protections using the link provided in the 4xTestProtections output. This will come in handy later in the workshop.

5. 5xTriggerRateLimiting will let you test the rate-limiting feature of AWS WAF later in the workshop.


#### Website Scanning Environment and Tools
This workshop provides two methods for testing AWS WAF rules that you will be implementing: automated and manual scanning. These scanners simulate common web attack vectors to help you identify and mitigate potential security risks, such as:

- SQL Injection (SQLi): Exploiting vulnerabilities to inject malicious code into database queries.
- Cross-site Scripting (XSS): Injecting malicious scripts into requests.
- Path Traversal: Accessing unauthorized files or directories on the web server.
- Sensitive Path Access: Attempting to access sensitive files or directories that should be restricted.
- Bot Activity: Identifying automated traffic and blocking malicious bots.
- API Misuse: Testing for improper usage of APIs.

These tests are designed to provide common scenarios for evaluating your workshop progress. Remember that for production deployments, it's crucial to conduct thorough security analysis and testing beyond the scope of this workshop.

Congratulations! You have now learned about the architecture used in this workshop, how to find relevant stack outputs, and basics about the scans performed against your website. Continue to the next section to learn more about our automated scanning and how to trigger additional scans on demand.


#### Automated Scanning
The Progress Dashboard provides a visual representation of your environment's security posture. It displays the number of automated tests (e.g., SQL injection, XSS) that are currently passing for your WAF configuration. The link to access this dashboard can be found within the Outputs tab of the CloudFormation stack.

Note that sometimes there may be a slight delay in data update. If you believe you have completed a task, but can't see it on the dashboard, please wait a minute or two and check again.

![dashboard](/images/2/dashboard.png)
Continue to the next section to learn how to trigger manual scanning of web application protections.


#### Manual Scanning
This section guides you through manually scanning your web application endpoint to assess the effectiveness of your WAF rules.

##### Running the Manual Scanner
Go to the Stack page, and open the link under the 4xTestProtections output in a new tab. This will perform scans against your web application endpoint. The scanner script runs tests and reports back the following information, such as:

Request: The HTTP request command used (for example GET or POST)
Result: The HTTP status code returned (for example 200 OK or 403 Forbidden)


The results are color-coded to help you quickly evaluate how your WAF rules are performing against their intended behavior. Ideally, all tests should return a green status code.

Baseline Requests represent legitimate traffic that should not be blocked by your WAF rules. They should always return a 200 OK status code to ensure you haven't unintentionally blocked valid users from accessing your application.

##### Reviewing the Scan Results
After running the scanner script, analyze the results to answer these questions:

Did any baseline tests get blocked? This indicates that your WAF rules might be too restrictive and need adjustment.
Were the simulated malicious requests successfully blocked? This shows that your WAF rules are effectively protecting your application.

##### Simulating Requests Yourself
The scanner script leverages an AWS Lambda function to send test requests. You can replicate this process by sending similar requests from your own device using tools like curl on the command line.

For very basic tests, you can even send simple GET requests directly from your browser, potentially with query string parameters, and compare the responses you receive.

The scan results will likely reveal security vulnerabilities that need to be addressed. The next step, remediation, involves configuring an AWS WAF Web ACL to block these malicious requests. When AWS WAF successfully blocks a request based on your rules, it returns a 403 Forbidden status code or a custom error code you've defined.

![manual_scanner](/images/2/manual_scanner.png)
Congratulations! Now that you're familiar with reviewing your progress, and testing applied protections, you're well-prepared to move on to the [Remediate](3-Remediate/) section. There, you'll explore strategies for implementing essential website protections with AWS WAF.