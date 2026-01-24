<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

---

# Load Data into DynamoDB

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-dynamodb)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## Project Architecture

![Project Architecture](https://learn.nextwork.org/projects/static/aws-databases-dynamodb/architecture-diagram.png)

---

## Load Data into a DynamoDB Table

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-dynamodb_b481c730)

---

## Introducing Today's Project!

### What is Amazon DynamoDB?

Amazon DynamoDB is a NoSQL database, specifically a key-value database. It is useful when prioritizing flexibility and speed in data management.

### How I used Amazon DynamoDB in this project

In today’s project, I used Amazon DynamoDB to upload data using AWS CloudShell to speed up the creation and loading of data into the tables.

### One thing I didn't expect in this project was...

One thing I didn’t expect in this project was how easy AWS CLI made it to create tables, manipulate data, and manage information

### This project took me...

This project took me 60 min

---

## Create a DynamoDB table

DynamoDB tables organises data using list of items (i.e. StudentNames, like Nikko), each with their own list of attributes (e.g. ProjectsComplete)

An attribute is like a piece of data about an item
Each item in DynamoDB can have multiple attributes. But, unlike relational databases where each row in a table must have the same columns, DynamoDB items can have their own unique set of attributes.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-dynamodb_a3cefee0)

---

## Read and Write Capacity

Read Capacity Units (RCUs) and Write Capacity Units (WCUs) are capacity units designed to measure and control read and write requests in a database. In Amazon DynamoDB, one RCU can handle one strongly consistent read request per second (or two eventually consistent reads), while one WCU can handle one write request per second.

The Free Tier for DynamoDB gives 25GB of data storage, plus 25 Write and 25 Read Capacity Units (WCU, RCU). This is enough to handle 200M requests per month

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-dynamodb_ef47dd8f)

---

## Using CLI and CloudShell

AWS CloudShell is a shell within the AWS Management Console that allows us to run commands and scripts.

AWS CLI (Command Line Interface) is a software that lets you create, delete and update AWS resources with commands instead of clicking through your console.

I ran a CLI command in AWS CloudShell that created four DynamoDB tables. The tables are as follows:

1- ContentCatalog Table: Contains a numeric attribute named Id.

2- Forum Table: Uses Name as the partition key.

3- Post Table: Uses ForumName as the partition key and Subject as the sort key.

4-Comment Table: Uses Id as the partition key and CommentDateTime as the sort key.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-dynamodb_81e0258b)

---

## Loading Data with CLI

I ran a CLI command in AWS CloudShell that Load the data of all four files into DynamoDB

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-dynamodb_791c600b)

---

## Observing Item Attributes

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-dynamodb_b481c731)

I checked a ContentCatalog item, which had the following attributes:

- Authors: A List containing one String value "Natasha"
- ContentType: String with value "Project"
- Difficulty: String with value "Easy peasy"
- Price: Number with value 0
- ProjectCategory: String with value "Storage"
- Published: Boolean set to True
- Title: String with value "Host a Website on Amazon S3"
- URL: String with value "aws-host-a-website-on-s3"

I checked a ContentCatalog item, which had the following attributes:

- Id: Number with value 203 (partition key)
- ContentType: String with value "Video"
- Price: Number with value 0
- Services: List (currently empty)
- Title: String with value "AWS x Databases project: Web App with Aurora Database"
- URL: String with value "https://youtube.com/live/PCyE_iU6rrU"
- VideoType: String with value "Live Project Demo"

---

## Benefits of DynamoDB

A benefit of DynamoDB over relational databases is flexibility, because each item can have its own set of attributes

Another benefit of DynamoDB over relational databases is speed, because it uses partition keys to split tables and quickly locate items. In contrast, relational databases often need to scan entire tables to find data, which can slow down performance

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-dynamodb_b481c730)

---
