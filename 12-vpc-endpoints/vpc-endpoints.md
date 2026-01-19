<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Endpoints

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-endpoints)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## Project Architeture

![Image](https://learn.nextwork.org/projects/static/aws-networks-endpoints/architecture-today.png)

---

## VPC Endpoints

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_09bcaa8a)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a logically isolated network used to group resources. It allows you to separate and organize resources that serve a similar purpose, giving you full control over networking, security, and traffic within AWS.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to connect to an S3 bucket through a VPC endpoint, so I can access it using the S3 private network instead of going through the public internet

### One thing I didn't expect in this project was...

One thing I didn’t expect in this project was that I needed to manually add the route to the VPC endpoint in the route table. I thought it would be automatic, but it turns out I had to select which route table to associate with the endpoint

### This project took me...

I completed this project in approximately 80 minutes.

---

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I will create a VPC using the “VPC and more” option to automatically create all required components. Then, I will launch an EC2 instance, connect to it using EC2 Instance Connect, and finally set up an S3 bucket.

### Step 2 - Connect to EC2 instance

In this step, I will connect to the EC2 instance and try to access S3 through the public internet.

### Step 3 - Set up access keys

In this step, I will create access key credentials to access AWS services.

### Step 4 - Interact with S3 bucket

In this step, I will use the EC2 instance to access the S3 bucket.

---

## Architecture set up

I started my project by creating a VPC using the “VPC and more” option, then launched an EC2 instance inside this VPC.

I also set up an S3 bucket named nextwork-vpc-project-ayoub and uploaded two files to it.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_4334d777)

---

## Access keys

### Credentials

To set up my EC2 instance to interact with AWS, I configured the access key ID, secret access key, the region to use, and the output format.

Access keys are credentials that allow applications and other servers to authenticate with AWS and interact with AWS services and resources.

Secret access keys are like passwords that pair with an access key ID; together, they are used to access AWS services.

### Best practice

Although I’m using access keys in this project, a best-practice alternative is to use IAM roles.

---

## Connecting to my S3 bucket

The command I ran was aws s3 ls, which enables me to list all the S3 buckets I have access to in my AWS account.

The terminal responded with a list of buckets in my AWS account. This indicated that the access keys I set up were working correctly.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_4334d778)

---

## Connecting to my S3 bucket

I also tested the command aws s3 ls s3://nextwork-vpc-project-ayoub, which returned the list of objects in the bucket nextwork-vpc-project-ayoub.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_4334d779)

---

## Uploading objects to S3

To upload a new file to my bucket, I first ran the command sudo touch /tmp/test.txt. This creates an empty file in the /tmp folder on my EC2 instance.

The second command I ran was aws s3 cp /tmp/test.txt s3://nextwork-vpc-project-ayoub. This copies the file test.txt from the /tmp folder on my EC2 instance to the S3 bucket nextwork-vpc-project-ayoub.

The third command I ran was aws s3 ls s3://nextwork-vpc-project-ayoub, which confirmed that the file test.txt was successfully copied.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_3e1e79a2)

---

## In the second part of my project...

### Step 5 - Set up a Gateway

In this step, I will Create an endpoint so the VPC and S3 communicate direclty zthout going though the public internet

### Step 6 - Bucket policies

In this step, i'll block all traffic to the S3 bucket except traffic coming from the endpoint. This will test whether the endpoint has been set up correctly.

### Step 7 - Update route tables

In this step, I will access the S3 bucket again to test whether the VPC endpoint is set up correctly

### Step 8 - Validate endpoint conection

In this step, I will test the VPC endpoint setup again after troubleshooting. Then, I will restrict my VPC's access to the AWS environment.

---

## Setting up a Gateway

I set up an S3 Gateway, which is a type of endpoint that works only with S3 and DynamoDB. It has limited security settings compared to an interface endpoint.

### What are endpoints?

An endpoint is a service that allows a VPC to communicate with an AWS service using the private network without going through the public internet.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_09bcaa8a)

---

## Bucket policies

A bucket policy is a set of permissions that grants or denies access to a bucket and its objects.

My bucket policy will deny all access to the bucket and its objects, except if it comes through the VPC endpoint I just created with this ID: vpce-0de551d4a9ac00cf5.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_7316a13d)

---

## Bucket policies

Right after saving my bucket policy, the S3 bucket page showed 'Access Denied' warnings. This happens because the policy blocks all actions unless they come through the VPC endpoint. As a result, any attempt to access the bucket from other sources, including the AWS Management Console, is blocked

I also had to update my route table to ensure that traffic going to the S3 bucket I created is routed through the VPC endpoint

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_4ec7821f)

---

## Route table updates

To update my route table, I added a route by going to the 'Change Route Table' option on the VPC endpoint page and selecting the route table to associate with it.

After updating my public subnet's route table, my terminal was able to list the objects in the nextwork-vpc-project-ayoub S3 bucket.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_d116818e)

---

## Endpoint policies

An endpoint policy is a set of rules attached to a VPC endpoint that controls which actions and resources can be accessed through that endpoint.

I updated my endpoint's policy by clicking the 'Edit Policy' button in the VPC endpoint's policy tab and changed the principal to 'Deny'. I could see the effect immediately, as access was denied when I tried to list the objects in the bucket again

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-endpoints_3e1e79a3)

---
