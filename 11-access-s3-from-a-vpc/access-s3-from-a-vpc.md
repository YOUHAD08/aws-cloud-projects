<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Access S3 from a VPC

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-s3)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## Project Architeture

![Image](https://learn.nextwork.org/projects/static/aws-networks-endpoints/architecture-past.png)

---

## Access S3 from a VPC

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-s3_3e1e79a2)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a logically isolated network in AWS that is used to group and isolate related resources.

### How I used Amazon VPC in this project

In today’s project, I used Amazon VPC to set up the entire network configuration, including the VPC, public subnet, network ACL, internet gateway, and route table, all at once using the VPC Wizard.

### One thing I didn't expect in this project was...

One thing I didn’t expect was that the same access key can be used to access AWS from different machines.

### This project took me...

This project took me probably 30 min

---

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I will create a VPC  
and launch an EC2 instance into this VPC

### Step 2 - Connect to my EC2 instance

In this step, I will connect directly to the EC2 instance.

### Step 3 - Set up access keys

In this step, I will create access keys to gave EC2 instance access to my AWS envirement

---

## Architecture set up

I started my project by launching and EC2 intance in the public subnet i created earlier

I also set up S3 bucket and uploaded two files

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-s3_4334d777)

---

## Running CLI commands

AWS CLI is a software that i install and run on the computer to control AWS services directly from the command line

The first command I ran was 'aws s3 list' This command is used to list all the s3 buckets in my accounts

The second command I ran was 'aws configue' This command is used to confugure the access keys

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-s3_e7fa8776)

---

## Access keys

### Credentials

To set up my EC2 instance to interact with my AWS environment, I configured the AWS CLI with my IAM credentials (access key ID and secret access key), along with the default region and output format. This allowed the EC2 instance to authenticate with AWS and make authorized API calls to AWS services such as EC2, S3, and IAM.

Access keys are credentials used by applications and servers to log into AWS and talk to my services and resources

Secret access keys are like password that pairs with access key ids to gave access to recources into my aws envirement

### Best practice

Although I'm using access keys in this project, a best practice alternative is to use IAM roles

---

## In the second part of my project...

### Step 4 - Set up an S3 bucket

In this step, I will create an S3 bucket After I'll learn how to access it from our EC2 instance .

### Step 5 - Connecting to my S3 bucket

In this step, I will Get the EC2 instance to interact with S3 bucket using access keys

---

## Connecting to my S3 bucket

The first command I ran was 'aws s3 list' This command is used to list all the s3 buckets in my accounts

When I ran the command 'aws s3 ls 'again, the terminal responded with list of buckets i have in my account This indicated EC2 have now access to my AWS envirement

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-s3_4334d778)

---

## Connecting to my S3 bucket

Another CLI command I ran was 'aws s3 ls s3://nextwork-vpc-project-ayoub' which returned content of the bucket
nextwork-vpc-project-ayoub

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-s3_4334d779)

---

## Uploading objects to S3

To upload a new file to my bucket, I first ran the command
sudo touch /tmp/test.txt to create a blank .txt file in EC2 instance

The second command I ran was.
aws s3 cp /tmp/test.txt s3://nextwork-vpc-project-
this command will move a file from EC2 to S3

The third command I ran was. aws S3 ls which validated that the file a moved is actualy

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-networks-s3_3e1e79a2)

---
