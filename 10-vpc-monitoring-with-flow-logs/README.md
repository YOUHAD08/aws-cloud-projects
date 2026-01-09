# VPC Monitoring with Flow Logs

## Project Overview

This project represents Part 7 in my AWS VPC series - **monitoring network traffic using VPC Flow Logs and CloudWatch**! Building on the VPC Peering skills from Part 6, I learned how to capture, store, and analyze network traffic data to gain visibility into my VPC's communication patterns. I configured Flow Logs to track all network activity, created IAM policies and roles for secure log delivery, and used CloudWatch Logs Insights to query and analyze traffic patterns.

**AWS Region Used:** eu-west-3 (Paris)  
**Project Series:** Part 7 of NextWork VPC Challenge

---

## Table of Contents

- [What I Built](#what-i-built)
- [Technologies & Concepts](#technologies--concepts)
- [Step-by-Step Implementation](#step-by-step-implementation)
  - [Step 1: Set Up Two VPCs Using VPC Wizard](#step-1-set-up-two-vpcs-using-vpc-wizard)
  - [Step 2: Launch EC2 Instances](#step-2-launch-ec2-instances)
  - [Step 3: Set Up Flow Logs](#step-3-set-up-flow-logs)
  - [Step 4: Set Up IAM Policy and Role for Flow Logs](#step-4-set-up-iam-policy-and-role-for-flow-logs)
  - [Step 5: Test VPC Peering and Generate Traffic](#step-5-test-vpc-peering-and-generate-traffic)
  - [Step 6: Create Peering Connection and Configure Routes](#step-6-create-peering-connection-and-configure-routes)
  - [Step 7: Analyze Flow Logs with CloudWatch Insights](#step-7-analyze-flow-logs-with-cloudwatch-insights)
- [Key Learnings](#key-learnings)
- [Cleanup](#cleanup)
- [Conclusion](#conclusion)
- [What's Next](#whats-next)

---

## What I Built

A complete VPC monitoring solution with Flow Logs and CloudWatch integration:

### **Architecture Highlights:**

**Key Achievement**: Successfully captured and analyzed network traffic patterns across two peered VPCs using VPC Flow Logs and CloudWatch Logs Insights!

**Components:**

- **Two VPCs** with unique CIDR blocks (10.1.0.0/16 and 10.2.0.0/16)
- **VPC Flow Logs** capturing all network traffic
- **CloudWatch Log Groups** storing flow log data
- **IAM Policy & Role** enabling Flow Logs to write to CloudWatch
- **VPC Peering Connection** for cross-VPC communication
- **EC2 Instances** in each VPC for traffic generation
- **CloudWatch Logs Insights** for traffic analysis

![Architecture](./images/00-architecture.png)

---

## Technologies & Concepts

### AWS Services Used

- **Amazon VPC** - Virtual Private Cloud for isolated networks
- **VPC Flow Logs** - Network traffic capture and monitoring
- **Amazon CloudWatch** - Log storage and monitoring service
- **CloudWatch Logs Insights** - Query and analyze log data
- **Amazon EC2** - Virtual servers for testing connectivity
- **VPC Peering** - Direct connection between VPCs
- **IAM Policies & Roles** - Permissions management
- **Route Tables** - Traffic routing configuration
- **Security Groups** - Instance-level firewall rules

### Key Concepts Learned

**VPC Flow Logs:**

- Captures information about IP traffic going to and from network interfaces in your VPC
- Records accepted and rejected traffic based on security group and NACL rules
- Can be published to CloudWatch Logs or S3
- Provides visibility for network monitoring, security analysis, and troubleshooting

**CloudWatch Integration:**

- Log Groups organize related log streams
- Log Streams contain flow log records for specific network interfaces
- Logs Insights provides powerful query capabilities
- Enables real-time monitoring and historical analysis

**IAM for Service Permissions:**

- Custom trust policies restrict which services can assume a role
- IAM policies define what actions a service can perform
- Separation of policies and roles provides fine-grained access control

---

## Step-by-Step Implementation

### Step 1: Set Up Two VPCs Using VPC Wizard

**Critical Rule**: VPCs must have unique CIDR blocks to avoid routing conflicts.

#### NextWork VPC 1

![Architecture](09-vpc-peering\images\03-nextwork-1-vpc-resources-map.png)

#### NextWork VPC 2

![Architecture](09-vpc-peering\images\06-nextwork-2-vpc-resources-map.png)

---

### Step 2: Launch EC2 Instances

**What I did:**

I launched one EC2 instance in each VPC to generate network traffic for monitoring.

#### Instance Configuration

**Instance 1 - NextWork VPC 1:**

```
Name: Instance - NextWork VPC 1
AMI: Amazon Linux 2023 AMI
Instance Type: t2.micro
VPC: NextWork-1-vpc
Subnet: Public subnet
Auto-assign Public IP: Enable
Key Pair: None (using EC2 Instance Connect)
```

**Security Group: NextWork-1-SG**

```
Inbound Rules:
- SSH (port 22) from 0.0.0.0/0
- All ICMP - IPv4 from 0.0.0.0/0
```

**Instance 2 - NextWork VPC 2:**

```
Name: Instance - NextWork VPC 2
AMI: Amazon Linux 2023 AMI
Instance Type: t2.micro
VPC: NextWork-2-vpc
Subnet: Public subnet
Auto-assign Public IP: Enable
Key Pair: None (using EC2 Instance Connect)
```

**Security Group: NextWork-2-SG**

```
Inbound Rules:
- SSH (port 22) from 0.0.0.0/0
- All ICMP - IPv4 from 0.0.0.0/0
```

#### Why Allow ICMP from All IP Addresses?

In the previous project, I learned to restrict ICMP traffic to specific VPC CIDR blocks. However, for comprehensive monitoring, I'm allowing ICMP from all sources:

1. **Testing public connectivity**: Validates instances are reachable from the internet
2. **Comparing traffic patterns**: Shows the difference between public and private communication
3. **Flow log variety**: Generates diverse traffic patterns for richer analysis
4. **Troubleshooting**: Enables ping tests from multiple sources

**Security Note**: In production, you'd restrict this to specific IP ranges.

---

### Step 3: Set Up Flow Logs

**What I did:**

I created a CloudWatch Log Group and configured VPC Flow Logs to capture all network traffic.

#### Creating CloudWatch Log Group

**What is CloudWatch?**

Amazon CloudWatch is AWS's monitoring and observability service. It collects and tracks metrics, logs, and events from AWS services and applications.

**Key Features:**

- **Real-time monitoring**: View metrics and logs as they happen
- **Dashboards**: Visualize data with customizable graphs
- **Alarms**: Automatically respond to metric thresholds
- **Logs**: Centralized storage for application and system logs

**What is a Log Group?**

A Log Group is like a folder that organizes related logs. It contains Log Streams, which are sequences of log events from the same source.

**My Log Group:**

```
Name: NextWorkVPCFlowLogsGroup
Retention: Never expire
Log Class: Standard
Region: eu-west-3
```

**Retention Setting**: "Never expire" means logs are kept indefinitely unless manually deleted. For production, you'd set a retention period (e.g., 30 days) to manage costs.

**Log Class**: Standard vs. Infrequent Access

- **Standard**: For logs accessed regularly, lower query costs
- **Infrequent Access**: For long-term archiving, lower storage costs but higher query costs

#### Configuring VPC Flow Logs

**What are VPC Flow Logs?**

VPC Flow Logs capture information about IP traffic going to and from network interfaces in your VPC. They're like a diary of all network conversations happening in your VPC.

**What Flow Logs Capture:**

- Source and destination IP addresses
- Source and destination ports
- Protocol (TCP, UDP, ICMP)
- Number of packets and bytes
- Action taken (ACCEPT or REJECT)
- Timestamp of traffic

**My Flow Log Configuration:**

```
Name: NextWorkVPCFlowLog
Filter: All (captures accepted and rejected traffic)
Maximum Aggregation Interval: 1 minute
Destination: CloudWatch Logs
Destination Log Group: NextWorkVPCFlowLogsGroup
```

**Filter Options:**

1. **All**: Captures all traffic (accepted + rejected)
2. **Accept**: Only traffic allowed by security groups and NACLs
3. **Reject**: Only traffic blocked by security groups and NACLs

**Why I chose "All"**: Provides complete visibility into all network activity, both successful and blocked.

**Aggregation Interval**: How often flow data is captured and logged

- **1 minute**: More granular, better for real-time monitoring
- **10 minutes**: Less frequent, lower storage costs

#### Understanding Network Interfaces

**What are Network Interfaces (ENIs)?**

An Elastic Network Interface (ENI) is a virtual network card that connects your EC2 instance to your VPC. It's what gives your instance its IP addresses and networking capabilities.

**ENI Properties:**

- Primary private IPv4 address
- One or more secondary private IPv4 addresses
- One Elastic IP address per private IPv4 address
- One public IPv4 address (optional)
- Security groups
- MAC address

**Why Flow Logs Track ENIs:**

Flow Logs are attached to network interfaces because each ENI represents a unique point where traffic enters or exits a resource. No two resources share the same ENI, making them perfect tracking points.

**Example:**
When I launched Instance 1, AWS automatically created an ENI with:

- Private IP: 10.1.x.x (from VPC 1's CIDR block)
- Public IP: [Elastic IP or auto-assigned]
- Attached Security Group: NextWork-1-SG

---

### Step 4: Set Up IAM Policy and Role for Flow Logs

**What I did:**

I created an IAM policy granting Flow Logs permission to write logs, and an IAM role that Flow Logs could assume to use these permissions.

#### Understanding IAM Policies vs. Roles

**The Challenge:**

VPC Flow Logs needs permission to:

1. Create log groups in CloudWatch
2. Create log streams within those groups
3. Write log events to the streams

But how do we give these permissions to a service (not a user)?

**Solution: IAM Roles**

**IAM Policy**: A document that defines permissions (what actions are allowed/denied)
**IAM Role**: A bundled set of policies that can be assumed by AWS services or users

**Analogy: Hotel Access System**

```
IAM Policies = Individual access rules
- "Can access gym"
- "Can access pool"
- "Can access executive lounge"
- "Can access room 305"

IAM Roles = Job titles with bundled access
- Guest Role: gym + pool + their assigned room
- Staff Role: all rooms + staff elevator + front desk computers
- Manager Role: all areas + security systems
```

You assign **roles** to people/services, not individual policies!

#### Creating the IAM Policy

**Policy Name**: NextWorkVPCFlowLogsPolicy

**JSON Policy Document:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ],
      "Resource": "*"
    }
  ]
}
```

**Policy Breakdown:**

| Element      | Purpose                                             |
| ------------ | --------------------------------------------------- |
| **Version**  | IAM policy language version (2012-10-17 is current) |
| **Effect**   | "Allow" grants permissions (vs. "Deny")             |
| **Action**   | Specific CloudWatch Logs API calls permitted        |
| **Resource** | "\*" means applies to all CloudWatch log resources  |

**Actions Explained:**

- `logs:CreateLogGroup`: Create new log groups
- `logs:CreateLogStream`: Create log streams within groups
- `logs:PutLogEvents`: Write log data to streams
- `logs:DescribeLogGroups`: View log group information
- `logs:DescribeLogStreams`: View log stream information

#### Creating the IAM Role with Trust Policy

**Role Name**: NextWorkVPCFlowLogsRole

**Why a Custom Trust Policy?**

Trust policies answer the question: "Who can assume this role?"

**The Problem:**

- VPC Flow Logs is a feature within the VPC service, not a standalone service
- The standard "AWS service" trusted entity dropdown doesn't have a Flow Logs option
- We need to explicitly allow only VPC Flow Logs to use this role

**My Custom Trust Policy:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "vpc-flow-logs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

**Trust Policy Breakdown:**

| Element       | Purpose                                             |
| ------------- | --------------------------------------------------- |
| **Principal** | Defines WHO can assume the role                     |
| **Service**   | Specifies AWS service (vpc-flow-logs.amazonaws.com) |
| **Action**    | sts:AssumeRole allows the service to use the role   |

**Security Benefit**: Even if this role is accidentally assigned to another service, that service can't use it because the trust policy locks it to VPC Flow Logs only!

#### Attaching Policy to Role

After creating both the policy and role, I attached `NextWorkVPCFlowLogsPolicy` to `NextWorkVPCFlowLogsRole`.

**Complete IAM Configuration:**

```
IAM Policy: NextWorkVPCFlowLogsPolicy
  ↓ (attached to)
IAM Role: NextWorkVPCFlowLogsRole
  ↓ (trusted by)
Service: VPC Flow Logs
```

Now Flow Logs has the permissions it needs to write to CloudWatch!

---

### Step 5: Test VPC Peering and Generate Traffic

**What I did:**

I connected to Instance 1 and attempted to ping Instance 2 to generate network traffic and test connectivity.

#### Initial Connectivity Test

**Connecting to Instance 1:**

```bash
# Using EC2 Instance Connect from AWS Console
# Successfully connected to Instance - NextWork VPC 1
```

**Testing Private IP Connectivity:**

```bash
# From Instance 1 terminal
$ ping 10.2.x.x
# No response - requests timing out
```

**Testing Public IP Connectivity:**

```bash
# From Instance 1 terminal
$ ping [Instance 2 public IP]
# Success! Receiving ping replies
```

#### Understanding the Problem

**Why private IP fails but public IP works:**

**Without VPC Peering:**

```
Instance 1 (VPC 1) --X--> Instance 2 (VPC 2) [Private IP]
  ❌ No direct route between VPCs

Instance 1 (VPC 1) --> Internet --> Instance 2 (VPC 2) [Public IP]
  ✅ Works via Internet Gateway
```

**Traffic Flow Analysis:**

1. **Private IP attempt** (10.2.x.x):

   - Instance 1 checks route table
   - No route to 10.2.0.0/16 found
   - Packets don't know where to go
   - Connection times out

2. **Public IP attempt**:
   - Instance 1 sends to 0.0.0.0/0 destination
   - Route table directs to Internet Gateway
   - Traffic goes over public internet
   - Reaches Instance 2's public IP
   - Success!

#### Route Table Investigation

**VPC 1 Route Table:**

```
Destination        Target
10.1.0.0/16       local
0.0.0.0/0         igw-xxxxx
```

**Missing**: Route to 10.2.0.0/16 via peering connection!

#### Testing from Local Terminal (Optional Extension)

**From my laptop:**

```bash
$ ping [Instance 2 public IP]
# Success! But with higher latency
time=147 ms (vs. 0.557 ms from Instance 1)
```

**Why the latency difference?**

- **Instance 1 to Instance 2**: Both in same AWS region (eu-west-3), physically close
- **My laptop to Instance 2**: Geographical distance, multiple network hops

**This test proves:**

1. Instance 2 is reachable from the internet (security group allows ICMP)
2. Private communication between VPCs requires peering
3. Proximity matters for latency

---

### Step 6: Create Peering Connection and Configure Routes

**What I did:**

I created a VPC peering connection and updated both VPCs' route tables to enable direct, private communication.

#### Creating VPC Peering Connection

**Peering Configuration:**

```
Name: VPC 1 <> VPC 2
Requester: NextWork-1-VPC (10.1.0.0/16)
Accepter: NextWork-2-VPC (10.2.0.0/16)
Region: This Region (eu-west-3)
Account: My Account
```

**Peering Process:**

1. **Request**: VPC 1 initiates peering connection
2. **Accept**: VPC 2 accepts the request
3. **Active**: Peering connection established

**Status After Acceptance**: Active ✅

#### Updating Route Tables

**VPC 1 Route Table Update:**

**Before:**

```
Destination        Target
10.1.0.0/16       local
0.0.0.0/0         igw-xxxxx
```

**After:**

```
Destination        Target
10.1.0.0/16       local
10.2.0.0/16       pcx-xxxxx (peering connection)
0.0.0.0/0         igw-xxxxx
```

**VPC 2 Route Table Update:**

**Before:**

```
Destination        Target
10.2.0.0/16       local
0.0.0.0/0         igw-xxxxx
```

**After:**

```
Destination        Target
10.2.0.0/16       local
10.1.0.0/16       pcx-xxxxx (peering connection)
0.0.0.0/0         igw-xxxxx
```

**Critical Point**: Peering is **bidirectional** - both VPCs need routes to each other!

#### Successful Connectivity Test

**After route table updates:**

```bash
# From Instance 1 terminal
$ ping 10.2.x.x
64 bytes from 10.2.x.x: icmp_seq=1 ttl=255 time=0.557 ms
64 bytes from 10.2.x.x: icmp_seq=2 ttl=255 time=0.612 ms
64 bytes from 10.2.x.x: icmp_seq=3 ttl=255 time=0.589 ms
# Success! Receiving replies via private IPs
```

**From local terminal** (should still timeout):

```bash
$ ping 10.2.x.x
# Request timeout
```

**Perfect!** This confirms:

1. ✅ VPC peering enables private communication
2. ✅ Private IPs are only accessible within AWS network
3. ✅ Peering doesn't expose private IPs to the internet

#### Advanced Ping Options

**Limiting ping packets:**

```bash
$ ping 10.2.x.x -c 5
# Sends exactly 5 ping packets, then stops
```

**What `-c 5` does:**

- Limits ping to a specific count
- Useful for consistent testing
- Provides summary statistics after completion

**Output Example:**

```
--- 10.2.x.x ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4003ms
rtt min/avg/max/mdev = 0.542/0.598/0.689/0.054 ms
```

---

### Step 7: Analyze Flow Logs with CloudWatch Insights

**What I did:**

I reviewed the captured flow logs in CloudWatch and used Logs Insights to query and analyze traffic patterns.

#### Reviewing Raw Flow Logs

**Navigating to Flow Logs:**

1. CloudWatch Console → Log groups
2. Select NextWorkVPCFlowLogsGroup
3. Click into the log stream (named eni-xxxxx)

**Why ENI naming?**

Log streams are named after network interfaces (Elastic Network Interface) because flow logs track traffic at the ENI level.

#### Understanding Flow Log Format

**Expanded Flow Log Example:**

```
2 123456789012 eni-abc123 18.237.140.165 10.1.5.112 57823 22 6 4 344 1673458742 1673458772 ACCEPT OK
```

**Field-by-field breakdown:**

| Field        | Value          | Meaning                      |
| ------------ | -------------- | ---------------------------- |
| version      | 2              | Flow log format version      |
| account-id   | 123456789012   | AWS account ID               |
| interface-id | eni-abc123     | Network interface ID         |
| srcaddr      | 18.237.140.165 | Source IP address            |
| dstaddr      | 10.1.5.112     | Destination IP address       |
| srcport      | 57823          | Source port                  |
| dstport      | 22             | Destination port (SSH)       |
| protocol     | 6              | Protocol number (6 = TCP)    |
| packets      | 4              | Number of packets            |
| bytes        | 344            | Number of bytes transferred  |
| start        | 1673458742     | Start timestamp (Unix epoch) |
| end          | 1673458772     | End timestamp (Unix epoch)   |
| action       | ACCEPT         | Traffic was allowed          |
| log-status   | OK             | Logging was successful       |

**What this log tells me:**

This flow log shows:

- **344 bytes** of data transferred
- From **18.237.140.165** (EC2 Instance Connect) to **10.1.5.112** (Instance 1)
- Using **TCP** protocol on **port 22** (SSH)
- **4 packets** were sent
- Traffic was **ACCEPTED** by security groups

**EC2 Instance Connect IP Range**: In the Oregon region, EC2 Instance Connect uses 18.237.140.160/29, confirming this is my connection!

#### Finding Rejected Traffic

**Looking for REJECT logs:**

Some logs show `REJECT OK` instead of `ACCEPT OK`.

**What this means:**

- Traffic was **blocked** by security group or NACL rules
- These are the failed ping attempts before I configured peering!
- Essential for security monitoring and troubleshooting

#### Using CloudWatch Logs Insights

**What is Logs Insights?**

CloudWatch Logs Insights is a query service that lets you search and analyze log data. Think of it as SQL for logs!

**Benefits:**

- Filter massive amounts of log data quickly
- Aggregate and calculate statistics
- Visualize results with charts
- Identify patterns and anomalies

**My Query Configuration:**

1. Navigate to Logs Insights
2. Select log group: NextWorkVPCFlowLogsGroup
3. Choose saved query: "Top 10 byte transfers by source and destination IP addresses"

#### Running the "Top 10 Byte Transfers" Query

**Query Purpose:**

Discovers the 10 biggest data transfers between IP addresses in my network.

**Use Cases:**

- **Identify heavy traffic flows**: Which connections transfer the most data?
- **Detect unusual activity**: Unexpected large transfers could indicate issues
- **Optimize network**: Understand traffic patterns for better routing
- **Cost analysis**: High-bandwidth connections may affect data transfer costs

**Query Syntax:**

```
fields @timestamp, srcAddr, dstAddr, bytes
| stats sum(bytes) as totalBytes by srcAddr, dstAddr
| sort totalBytes desc
| limit 10
```

**Query Breakdown:**

| Line                                                 | Purpose                           |
| ---------------------------------------------------- | --------------------------------- |
| `fields @timestamp, srcAddr, dstAddr, bytes`         | Select relevant fields from logs  |
| `stats sum(bytes) as totalBytes by srcAddr, dstAddr` | Calculate total bytes per IP pair |
| `sort totalBytes desc`                               | Sort by highest traffic first     |
| `limit 10`                                           | Show only top 10 results          |

**Query Results:**

The results table showed:

- **srcAddr**: Source IP address
- **dstAddr**: Destination IP address
- **totalBytes**: Total data transferred between them

**Example Results:**

```
srcAddr          dstAddr         totalBytes
18.237.140.165   10.1.5.112     12,456
10.1.5.112       10.2.15.210    8,932
...
```

**What I discovered:**

1. EC2 Instance Connect to Instance 1 (SSH connection) transferred the most data
2. Instance 1 to Instance 2 (ping tests via peering) is second
3. All IP addresses match my test infrastructure

**Visualization:**

The bar chart at the top shows log volume over time:

- X-axis: Time intervals
- Y-axis: Number of log entries
- Shows when I generated the most traffic (during ping tests)

---

## Key Learnings

### 1. Network Monitoring is Essential

**Why monitor networks?**

Network monitoring helps you:

- **Validate configuration**: Confirm resources are communicating as expected
- **Detect security threats**: Identify unauthorized access attempts
- **Troubleshoot issues**: Pinpoint connectivity problems quickly
- **Optimize performance**: Find bottlenecks and inefficiencies
- **Meet compliance**: Maintain audit trails for regulatory requirements

**Flow Logs provide:**

- Complete traffic visibility
- Security analysis capabilities
- Historical traffic data
- Real-time monitoring potential

### 2. IAM Service Permissions

**Key Concept**: AWS services need IAM roles to interact with other services.

**The Three-Step Process:**

1. **Create IAM Policy**: Define what actions are allowed
2. **Create IAM Role**: Bundle policies and add trust policy
3. **Assign Role**: Give the role to the service that needs it

**Trust Policies are critical:**

- They specify WHO can use a role
- Prevent accidental misuse
- Provide an extra security layer

**Lesson learned**: Always use custom trust policies for specific services that aren't in the standard dropdown.

### 3. Flow Logs Capture Everything

**What gets logged:**

- ✅ Accepted traffic (allowed by security groups/NACLs)
- ✅ Rejected traffic (blocked by security groups/NACLs)
- ✅ All protocols (TCP, UDP, ICMP, etc.)
- ✅ All ports
- ✅ Internal VPC traffic
- ✅ Traffic to/from internet

**What doesn't get logged:**

- ❌ Traffic to Amazon DNS servers
- ❌ Traffic to Amazon Windows license servers
- ❌ Traffic to/from instance metadata service (169.254.169.254)
- ❌ DHCP traffic
- ❌ Traffic to reserved IP addresses

### 4. Public vs. Private Communication

**Comparison:**

| Aspect            | Public IP             | Private IP             |
| ----------------- | --------------------- | ---------------------- |
| **Accessibility** | Internet              | AWS network only       |
| **Security**      | Exposed               | Protected              |
| **Latency**       | Higher                | Lower                  |
| **Cost**          | Data transfer charges | Free (same region)     |
| **Use Case**      | External access       | Internal communication |

**Best Practice**: Use private IPs with VPC peering for secure, cost-effective inter-VPC communication.

### 5. CloudWatch Logs Insights is Powerful

**Query capabilities:**

- Filter by any field (IP, port, action, protocol)
- Aggregate data (sum, count, average)
- Time-based analysis
- Pattern detection
- Custom visualizations

**Use Cases:**

- Security analysis: Find blocked connection attempts
- Capacity planning: Identify bandwidth-heavy applications
- Cost optimization: Analyze data transfer patterns
- Compliance reporting: Generate audit trails

### 6. Aggregation Intervals Matter

**1-minute intervals:**

- ✅ More granular data
- ✅ Better for real-time monitoring
- ✅ Easier to pinpoint exact events
- ❌ Higher log volume
- ❌ Higher storage costs

**10-minute intervals:**

- ✅ Lower log volume
- ✅ Lower costs
- ❌ Less granular
- ❌ Harder to pinpoint exact timing

**My choice**: 1-minute for this learning project. Production choice depends on monitoring needs vs. budget.

### 7. Log Retention Strategy

**Options:**

- Never expire (default)
- 1 day to 10 years

**Considerations:**

- **Compliance requirements**: Some industries require specific retention periods
- **Cost**: Longer retention = higher storage costs
- **Use case**: Active monitoring vs. long-term archiving

**Best Practice**: Set retention based on actual needs. Don't store logs "just in case" without a plan.

---

## Cleanup

**Important**: Delete resources in the correct order to avoid dependency errors!

### Deletion Order:

1. **CloudWatch Log Group**

   - CloudWatch Console → Log groups
   - Select NextWorkVPCFlowLogsGroup
   - Actions → Delete log group(s)

2. **EC2 Instances**

   - EC2 Console → Instances
   - Select both instances
   - Instance state → Terminate instance

3. **VPC Peering Connection**

   - VPC Console → Peering connections
   - Select VPC 1 <> VPC 2
   - Actions → Delete peering connection
   - ☑️ Delete related route table entries

4. **VPCs** (cascade deletes subnets, route tables, IGWs, NACLs, security groups)

   - VPC Console → Your VPCs
   - Select NextWork-1-vpc
   - Actions → Delete VPC
   - Repeat for NextWork-2-vpc

5. **IAM Role and Policy**
   - IAM Console → Roles
   - Search "FlowLogs", delete NextWorkVPCFlowLogsRole
   - IAM Console → Policies
   - Search "FlowLogs", delete NextWorkVPCFlowLogsPolicy

**Verification**: Refresh each page to confirm resources are deleted.

**Tip**: If VPC deletion fails due to attached network interfaces, manually delete ENIs first (rare, but possible).

---

## Conclusion

This project took my VPC skills to the next level by adding monitoring capabilities!

**Major Achievements:**

1. ✅ **Configured VPC Flow Logs** - Captured all network traffic going in and out of VPC 1
2. ✅ **Integrated with CloudWatch** - Stored logs in a centralized location for analysis
3. ✅ **Created IAM Policy and Role** - Granted Flow Logs the permissions needed to write logs
4. ✅ **Implemented Custom Trust Policy** - Restricted role usage to VPC Flow Logs only
5. ✅ **Set up VPC Peering** - Connected two VPCs for private communication
6. ✅ **Generated Test Traffic** - Used ping to create various traffic patterns
7. ✅ **Analyzed with Logs Insights** - Queried logs to discover top traffic sources
8. ✅ **Understood Flow Log Format** - Can interpret raw flow log data
9. ✅ **Differentiated Traffic Types** - Recognized accepted vs. rejected traffic
10. ✅ **Compared Public vs. Private Communication** - Validated peering benefits

---

## What's Next?

This project is **Part 7** of the NextWork VPC series. In the next project, I'll explore:

**Part 8: Access S3 from a VPC**

What I'll learn:

- Connect VPC to Amazon S3 service
- Configure VPC Gateway Endpoints
- Understand public vs. private service access
- Route traffic to AWS services without going over the internet
- Reduce data transfer costs with VPC endpoints
- Enhance security by keeping traffic within AWS network

---

**Project Completed:** January 2026  
**Author:** YOUHAD AYOUB  
**Region:** eu-west-3 (Paris)  
**NextWork Challenge:** AWS Beginners Challenge - Project 7
