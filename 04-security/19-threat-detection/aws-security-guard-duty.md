<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Threat Detection with GuardDuty

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-guardduty)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-security-guardduty/architecture-complete.png)

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_v1w2x3y4)

---

## Introducing Today's Project!

### Tools and concepts

Key services and concepts learned in this project:

OWASP Juice Shop – practice exploiting web app vulnerabilities.

Amazon EC2 & Networking – instance roles, VPCs, and security groups.

Amazon S3 – storing sensitive data securely.

AWS CloudFormation – deploying infrastructure as code.

Amazon GuardDuty – detecting threats, credential misuse, and malware.

CloudShell & Temporary Credentials – simulating attacks safely.

Security Attacks – SQL injection, command injection, and credential exfiltration.

Malware Protection – detecting malicious files (e.g., EICAR test files).

This project taught me how attacks happen, how to detect them, and how to secure cloud resources.

### Project reflection

This project took me approximately 60 min

I did this project to practice cloud security, detect threats, and understand attacks in a safe environment.

Yes, it met my goals by showing how vulnerabilities can be exploited and how GuardDuty protects AWS resources.

---

## Project Setup

To set up this project, I deployed an AWS CloudFormation template that automatically launches the resources required to build the web application.

The infrastructure consists of three main components:

Web Infrastructure – This includes compute and networking resources such as an Amazon EC2 instance, a Amazon VPC with subnets and security groups, as well as scaling and traffic management services like AWS Auto Scaling and Elastic Load Balancing.

Storage Layer – An Amazon S3 bucket is used to store sensitive data that the project aims to protect.

Security Monitoring – Amazon GuardDuty is enabled to detect potential security threats and suspicious activity using machine learning and threat intelligence.

Together, these components create a web environment where I can simulate attacks and learn how to detect and secure cloud resources.

To practice my skills with Amazon GuardDuty, I will attempt to attack this application and observe whether GuardDuty detects the suspicious activities and how it responds to them.

Amazon GuardDuty is a threat detection service from AWS that continuously monitors your AWS environment for suspicious or malicious activity.

It analyzes data from sources such as VPC Flow Logs, CloudTrail events, and DNS logs to identify potential threats like:

1-Unauthorized access attempts

2-Compromised EC2 instances

3-Suspicious API activity

Communication with known malicious IP addresses

GuardDuty uses machine learning and threat intelligence to detect these risks and generate security findings so you can investigate and respond quickly.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_n1o2p3q4)

---

## SQL Injection

The first attack I performed on the web application was SQL injection. This technique involves inserting malicious SQL code into an application's database query.

SQL Injection is a major security risk because it can allow attackers to bypass authentication, access sensitive data, or even modify the database structure.

My SQL injection attack involved entering ' OR 1=1;-- in the login form. This input makes the database query always evaluate to true, allowing me to bypass the authentication check and log in without valid credentials.

This is a common example of SQL Injection, where attackers manipulate database queries to gain unauthorized access. 🔐

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_h1i2j3k4)

---

## Command Injection

Next, I used command injection, which is an attack where an attacker inserts malicious system commands into an application so the server executes them.

The web app OWASP Juice Shop is vulnerable to this because it intentionally contains insecure input handling that does not properly validate or sanitize user input. This allows attackers to run commands on the underlying system.

To perform the command injection attack, I inserted the following payload into the username field of the login form.

This script forces the server to execute system commands that query the EC2 instance metadata service at 169.254.169.254. The metadata service returns the temporary IAM credentials attached to the instance.

The command then saves those credentials into a file inside the web application's public directory so they can be accessed from the browser.

This works because the application OWASP Juice Shop is intentionally vulnerable and does not properly sanitize user input, allowing injected commands to run on the server. The credentials retrieved come from the Amazon EC2 instance metadata service.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_t3u4v5w6)

---

## Attack Verification

To verify the attack worked, I visited the endpoint where the credentials were stored:
https://du46s49pnpsv1.cloudfront.net/assets/public/credentials.json.

The page showed a JSON file with AWS credentials, confirming the attack was successful.

These credentials are the web app’s keys to access AWS resources in the account. If an attacker gets them, they could access data, modify resources, or create new services and cause costs.

The file included:

AccessKeyId and SecretAccessKey – the main AWS access keys

Token – a temporary session token

Expiration – when the credentials stop working

These credentials come from the \*\*Amazon EC2 instance role used by the application.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_x7y8z9a0)

---

## Using CloudShell for Advanced Attacks

The attack continues in AWS CloudShell, because it lets me simulate a hacker using the stolen AWS credentials from outside the original environment.

Since CloudShell runs in a different AWS account context, using the stolen credentials there appears suspicious. This helps Amazon GuardDuty detect unusual activity, such as another account using credentials that belong to the Amazon EC2 instance.

In AWS CloudShell, I used wget to download the credential file into my CloudShell environment.

Next, I ran a command using cat and jq to display the credentials in a clean and organized JSON format, making them easier to read and analyze.

I then set up a profile called stolen to use the credentials extracted from the attack on the web application. I created a new profile because AWS CloudShell normally uses the default profile, which inherits the permissions of the user logged into the AWS Management Console.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_j9k0l1m2)

---

## GuardDuty's Findings

After performing the attack, Amazon GuardDuty reported a finding within 2 minutes.

A finding is a notification that suspicious activity has been detected in your AWS environment. It explains what happened, who performed the action, and how the activity occurred.

The Amazon GuardDuty finding was called UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS.

This means someone inside AWS tried to use credentials assigned to an Amazon EC2 instance in a suspicious way. GuardDuty detected this using anomaly detection, which identified unusual use of the instance’s credentials.

Amazon GuardDuty reported a high‑severity finding called UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS.

The finding showed that credentials from an Amazon EC2 instance were used to access data in an Amazon S3 bucket (nextwork-guardduty-project-ayoub-thesecurebucket-y9moko8wnzeb).

GuardDuty detected a GetObject API call from an external AWS account and IP address, which attempted to retrieve data from the bucket. Because these credentials normally belong to the EC2 instance, this unusual usage triggered the alert.

This confirmed that the stolen instance credentials were used to access sensitive data, which GuardDuty correctly flagged as suspicious activity.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_v1w2x3y4)

---

## Extra: Malware Protection

For my project extension, I enabled malware protection for S3.

Malware is software designed to damage, disrupt, or gain unauthorized access to computer systems and data. This allows GuardDuty to detect and alert on malicious files uploaded to the bucket.

To test GuardDuty’s malware detection, I uploaded an EICAR test file to the S3 bucket.

Once I uploaded the file, Amazon GuardDuty instantly triggered a malware finding.

This verified that malware protection is working correctly and GuardDuty can detect and alert on suspicious or malicious files in the S3 bucket.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-guardduty_sm42x3y4)

---
