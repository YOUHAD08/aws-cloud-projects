<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Security Monitoring System

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-monitoring)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-security-monitoring/architecture-complete.png)

##

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-monitoring_reghtjy)

---

## Introducing Today's Project!

In this project, I will be setting up a monitoring system using AWS CloudTrail, CloudWatch, and SNS.

This is how it works:

🛠 Set up AWS CloudTrail to track secret access events.

🔎 Use AWS CloudWatch to log access attempts and trigger notifications.

🔔 Create SNS alerts to get notified whenever your secrets are accessed.

💎 Build a second notification system and compare which approach provides better security alerts.

### Tools and concepts

The key services and concepts I learned in this project include:

- AWS CloudTrail – Used to record and track API activity in my Amazon Web Services account, helping monitor actions such as retrieving secrets.

- Amazon CloudWatch – Used to analyze logs, create metric filters, and configure alarms for monitoring specific events.

- Amazon Simple Notification Service – Used to send email notifications when a CloudWatch alarm is triggered.

- AWS Secrets Manager – Used to securely store and retrieve secrets such as API keys or credentials.

I also learned important monitoring concepts such as log analysis, metric filters, alarm thresholds, event tracking, and automated alerting, which are essential for building secure and observable cloud systems.

### Project reflection

This project took me approximately. 60 min

---

## Create a Secret

AWS Secrets Manager is a service used to securely store and manage secrets, such as API keys, passwords, and database credentials. It also allows applications to safely access these secrets through code.

To set up my project, I created a secret called TopSecretInfo that contains dummy secret data used only for testing purposes.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-monitoring_o5p6q7r8)

---

## Set Up CloudTrail

AWS CloudTrail is a monitoring service that records different activities occurring in your Amazon Web Services account. It keeps a log of who did what and when an event happened, such as API calls, accessing secrets, modifying settings, and more.

To do this, I set up a trail to record specific activities and store the log data in a specified location, such as an Amazon S3 bucket.

AWS CloudTrail events include several types:

Management events: These record administrative actions used to configure your Amazon Web Services resources.

Data events: These log resource operations performed on or within a resource, such as accessing data in Amazon S3 .

Insights events: These help detect unusual activity or abnormal behavior in your account.

Network activity events: These track events that occur at the network level, helping monitor how resources are accessed over the network.

### Read vs Write Activity

Read API activity involves retrieving information without making any changes to the resource.

Write API activity involves actions that modify a resource, such as creating, updating, or deleting it.

For this project, we need to track the retrieval of a secret key. Interestingly, in AWS Secrets Manager, this action is recorded as a Write API activity in AWS CloudTrail for security reasons.

---

## Verifying CloudTrail

I retrieved the secret in two ways:

First, through the Amazon Web Services Management Console by searching for my secret and clicking the “Retrieve secret value” button, which triggers an API call to access the secret.

Second, using the AWS Command Line Interface by running the following command:

aws secretsmanager get-secret-value --secret-id "TopSecretInfo" --region eu-west-3

Both methods trigger events that are logged by AWS CloudTrail.

To analyze my AWS CloudTrail events, I visited the service in the Amazon Web Services Management Console, specifically the Event History section. I found many events, so to locate the relevant ones related to my secret, I filtered the results by Resource type: AWS Secrets Manager.

After applying the filter, I found the GetSecretValue event that was recorded recently. The event details included important metadata such as the timestamp of the event, the resource used, and the name of the user that performed the action.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-monitoring_s8t9u0v1)

---

## CloudWatch Metrics

Amazon CloudWatch Logs is a service used to collect logs from different AWS services. It is used for visibility, troubleshooting, and monitoring of applications and infrastructure.

CloudTrail’s Event History is useful because it allows us to quickly investigate events in our account.

CloudWatch Logs, on the other hand, are better for creating metrics, setting up alarms, and automating responses. They also allow long-term storage of logs, unlike CloudTrail, which retains events for only 90 days. Additionally, CloudWatch provides advanced filtering capabilities, which help a lot in analyzing large volumes of log data.

A CloudWatch metric is a quantitative measurement of a specific activity or event in your AWS environment.

When setting up a metric:

1- The metric value represents what is recorded whenever the filter detects a match in the logs.

2- A default value is used when the filter does not record any match during a given period of time.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-monitoring_a9b0c1d2)

---

## CloudWatch Alarm

A CloudWatch alarm is a monitoring tool that watches a specific metric and triggers a response when the metric crosses a defined threshold.

I set my CloudWatch alarm threshold to 1, so the alarm will trigger whenever the SecretIsAccessed metric exceeds this value, sending notifications through the configured SNS topic.

I created an Amazon SNS topic along the way.

An SNS (Simple Notification Service) topic is like a broadcast channel for your notifications. First, you create the channel (the topic), then you add subscribers (such as your email), and finally, you send messages to the topic. SNS automatically delivers the message to all subscribers, ensuring everyone receives the notification.

AWS requires email confirmation to ensure that it is really me requesting to receive messages from the SNS topic. This helps prevent unauthorized subscriptions and ensures that notifications are sent only to intended recipients.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-monitoring_fsdghstt)

---

## Troubleshooting Notification Errors

To test my monitoring system, I tried to access the secret key again. The results were surprising, because I did not receive any notification from the CloudWatch alarm.

When troubleshooting the notification issue, there are several places in the monitoring system where things could be failing:

1- AWS CloudTrail might not have recorded the GetSecretValue event.

2- CloudTrail might not be sending logs to Amazon CloudWatch Logs.

3- The CloudWatch metric filter might not be filtering the logs correctly.

4- The Amazon CloudWatch alarm might not be triggering an action.

5- Amazon Simple Notification Service (SNS) might not be delivering the email notifications.

Each of these components must be configured correctly for the monitoring and alerting pipeline to work end-to-end.

I initially didn’t receive an email because of the alarm configuration. The statistic function was not set correctly. I changed it to Sum instead of Average, and I also reduced the evaluation period from 5 minutes to 1 minute.

---

## Success!

To validate that my monitoring system works, I checked the SecretIsAccessed alarm.

It s in the In Alarm state. I also received a notification email, confirming that the \*\*Amazon CloudWatch alarm was triggered and the alert was successfully sent through Amazon Simple Notification Service.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-monitoring_ageraergearge)

---

## Comparing CloudWatch with CloudTrail Notifications

In a project extension, I updated the CloudTrail configurations to observe what notifications I would receive and when, and then compared them to the notifications from the CloudWatch alarm to evaluate which approach provides faster or more informative alerts.

After enabling CloudTrail SNS notifications, my inbox started receiving multiple notifications each time a file was pushed to S3 or other management events occurred in my account.

I found this insufficient, because the notifications were too broad and not specific to the events I actually wanted to monitor

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-monitoring_d7e8f9g0)

---

---
