<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Query Data with DynamoDB

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-query)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## Project Architecture

![Project Architecture](https://learn.nextwork.org/projects/static/aws-databases-dynamodb/architecture-diagram.png)

---

## Query Data with DynamoDB

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-query_733d9399)

---

## Introducing Today's Project!

### What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed NoSQL database service from AWS that stores data in a flexible, key-value format instead of traditional rows and columns. It's useful because it's extremely fast (millisecond response times), automatically scales to handle any amount of traffic, and requires minimal setup or maintenance since AWS manages all the infrastructure for you. This makes it perfect for applications that need high performance and reliability, like mobile apps, gaming leaderboards, shopping carts, or any system that handles unpredictable or rapidly growing amounts of data without you having to worry about managing servers or database performance

### How I used Amazon DynamoDB in this project

In today’s project, I used Amazon DynamoDB to create tables and load data using AWS CloudShell. I then ran queries using the AWS Console and AWS CLI, and finally executed a transaction.

### One thing I didn't expect in this project was...

One thing I didn’t expect in this project is that transactions can only be run using the AWS CLI, not the AWS Console.

### This project took me...

This project took me 60 min

---

## Querying DynamoDB Tables

A partition key is used to organize data into partitions, making it easier to filter and find specific items in the table

A sort key is a second key used alongside the partition key to further filter and find specific items in the table.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-query_d105b0b0)

---

## Limits of Using DynamoDB

I got an error when querying for all comments by User Abdulrahman because I didn't include a partition key in the query

Insights we could extract from our Comment table includes the identity of your most frequent contributors , such as "User Abdulrahman" and "User Abhishek," and the specific topics generating the most buzz , like "Project #7" or "IAM User Setup." You can also determine feedback timing and velocity by analyzing the CommentDateTimetimestamps to see when engagement peaks. Finally, the table provides a clear look at general user sentiment , which in this case is overwhelmingly positive with descriptors like "Legendary" and "Excellent."

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-query_cb3e260c)

---

## Running Queries with CLI

A query I ran in CloudShell was...
bashaws dynamodb get-item \
 --table-name ContentCatalog \
 --key '{"Id":{"N":"101"}}' \
 --consistent-read \
 --projection-expression "Title, ContentType, Services" \
 --return-consumed-capacity TOTAL
This query will...
Retrieve the item with Id 101 from the ContentCatalog table, returning only the Title, ContentType, and Services attributes using a strongly consistent read, and display the total read capacity units consumed by this operation.

1. --consistent-read
   Gets you the guaranteed latest version of the data (costs more, but 100% up-to-date).

2. --projection-expression "Title, ContentType, Services"
   Only returns these 3 specific fields instead of everything.

3. --return-consumed-capacity TOTAL
   Shows how many Read Capacity Units this request used (helps track costs).

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-query_733d9399)

---

## Transactions

A transaction is a group of operations that all have to succeed - if any of the operations in the group fails, none of the changes get applied

I ran a transaction using aWScloudshell This transaction did two things :

1 - add a new Comment to the Comment table.

2- increase the Comments count in the related Forum item.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-query_2f65f83e)

---
