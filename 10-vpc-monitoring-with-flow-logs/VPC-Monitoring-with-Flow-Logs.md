<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Monitoring with Flow Logs

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-monitoring)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## VPC Monitoring with Flow Logs

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_3e1e79a1)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a private network in AWS where you can run your resources. It’s useful because it controls security, networking, and isolation, letting you manage IPs, subnets, and connections safely.

### How I used Amazon VPC in this project

In today’s project, I used Amazon VPC to test VPC peering and CloudWatch to monitor and analyze flow logs.

### One thing I didn't expect in this project was...

One thing I didn’t expect in this project was that I could use VPC peering to connect VPCs from different accounts and regions. I also learned that I need to assign roles to flow logs, so they can write and send logs to a log group for analysis

### This project took me...

This project took me about 1 hour

---

## In the first part of my project...

### Step 1 - Set up VPCs

In this step, I will set up two VPCs using VPC Wizard 

### Step 2 - Launch EC2 instances

In this step, I will launch one EC2 instance in each VPC so that I can use them later to test the VPC peering connection.

### Step 3 - Set up Logs

In this step, I will enable VPC Flow Logs to record all incoming and outgoing network traffic in the VPC. These logs will be saved in a dedicated storage location so they can be reviewed later for security and connectivity analysis.

### Step 4 - Set IAM permissions for Logs

In this step, I will grant VPC Flow Logs the necessary permissions to write logs and send them to CloudWatch Logs. This is required to complete the setup of the subnet’s flow logs.

---

## Multi-VPC Architecture

I started my project by creating two VPCs, each containing one public subnet.

The CIDR blocks for VPC 1 and VPC 2 are 10.1.0.0/16 and 10.2.0.0/16, respectively. These CIDR blocks must be unique to ensure that the IP addresses of resources do not overlap. Overlapping IP ranges can lead to routing conflicts and connectivity issues.

### I also launched EC2 instances in each subnet

My EC2 instances’ security groups allow SSH access from all sources (0.0.0.0/0) to enable connections to the EC2 instances using EC2 Instance Connect. I also allowed ICMP traffic from anywhere so that ping requests and responses can be tested.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_e7fa8775)

---

## Logs

Logs are like a diary of a system or an application. They record every action performed on the system, which helps us understand how it is functioning and makes troubleshooting easier when an error or issue occurs.

Log groups are used to collect and organize related logs in one place so they are easier to manage and analyze.

### I also set up a flow log for VPC 1

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_e8398869)

---

## IAM Policy and Roles

I created an IAM policy to grant VPC Flow Logs the required permissions to create log streams and send flow log data to a CloudWatch log group.

I also created an IAM role to assign VPC Flow Logs the correct permissions needed to create and access log resources.

A custom trust policy is a policy that grants permission to a specific service or entity within a service to assume an IAM role. 

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_4334d777)

---

## In the second part of my project...

### Step 5 - Ping testing and troubleshooting

In this step, I will test the VPC peering connection by verifying that the EC2 instance in VPC 1 can send a message to the EC2 instance in VPC 2.

### Step 6 - Set up a peering connection

In this step, I will create a VPC peering connection and configure the route tables to enable communication between the two VPCs.

### Step 7 - Analyze flow logs

In this step, I will review the flow logs for VPC 1's public subnet and analyze them to gain useful insights about the network traffic.

---

## Connectivity troubleshooting

My first ping test between the EC2 instances received no replies, indicating that there is an issue with the connection.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_99d4ba42)

I was able to receive ping replies when using the other instance's public IP address. This indicates that Instance 2 is correctly configured to respond to ping requests, and Instance 1 can communicate with Instance 2 when the traffic goes over the public internet.

---

## Connectivity troubleshooting

Looking at VPC 1's route table, I found that the ping test using Instance 2's private IP failed because the connection between the two VPCs is not configured through VPC peering.

### To solve this, I set up a peering connection between my VPCs

I also updated the route tables for both VPCs so that traffic destined for the other VPC is routed through the VPC peering connection.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_7316a13d)

---

## Connectivity troubleshooting

I received ping replies from Instance 2's private IP address! This means that the VPC peering between the two VPCs is set up correctly, and the connection is now going through AWS’s private network instead of the public internet.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_4ec7821f)

---

## Analyzing flow logs

A VPC flow log record contains several parts: the version of the log format, the AWS account ID that created the log, the network interface ID, the source and destination IP addresses, the source and destination ports, the protocol used (e.g., TCP), the number of packets and bytes transferred, the start and end times of the traffic session, the action taken (ACCEPT or REJECT), and the log status indicating whether the record was captured successfully

This AWS VPC Flow Log shows network traffic for a single network interface on January 18, 2026, capturing about 2 hours of activity with a mix of accepted and rejected connections using common protocols like HTTP (port 80), HTTPS (port 443), and NTP (port 123). The traffic volumes are relatively small, ranging from a few packets to a few hundred per flow, with data transfers mostly in the kilobyte range. Overall, it appears to be normal, routine network activity with security rules functioning properly—allowing legitimate traffic while blocking some unwanted connections, and there are no obvious signs of attacks or suspicious behavior.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_d116818e)

---

## Logs Insights

AWS CloudWatch Logs Insights lets you search, analyze, and visualize log data stored in CloudWatch Logs. It allows you to run queries to filter, aggregate, and find patterns in your logs quickly. You can also create charts and dashboards from the results to monitor applications and troubleshoot issues efficiently.

I ran the query "stats sum(bytes) as bytesTransferred by srcAddr, dstAddr
| sort bytesTransferred desc
| limit 10"
The query sums the total bytes transferred between each source and destination address, sorts the results from highest to lowest, and shows only the top 10 pairs.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-monitoring_3e1e79a1)

---

---
