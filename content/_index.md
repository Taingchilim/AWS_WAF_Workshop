+++
title = "AWS Web Application Firewall"
date = 2020
weight = 1
chapter = false
+++

# What is AWS WAF?

AWS WAF is a web application firewall service. It helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources.

Using a WAF is a great way to add defense in depth to your web application. A WAF can help mitigate the risk of vulnerabilities such as [SQL Injection](https://www.owasp.org/index.php/SQL_Injection), [Cross Site Scripting](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)) and other common attacks (which listed in Top 10 OWASP). WAF allows you to create your own custom rules to decide whether to block or allow HTTP requests before they reach your application.

#### Contents

We devided this lab into sections below:

1. [Prerequiste](1-prerequiste/)
2. [Web ACLs and Managed Rules](2-acls-managed-rules/)
3. [Custom Rules](3-custom-rules/)
4. [Advanced Custom Rules](4-advanced-rules/)
5. [Testing New Rules](5-testing/)
6. [Logging](6-logging/)
7. [Cleanup](7-clean-up)