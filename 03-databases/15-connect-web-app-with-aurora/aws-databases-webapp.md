<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

---

# Connect a Web App with Aurora

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-webapp)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## Project Architecture

![Project Architecture](https://learn.nextwork.org/projects/static/aws-databases-webapp/architecture-diagram5.png)

---

## Connect a Web App to Amazon Aurora

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-webapp_1709b26b)

---

## Introducing Today's Project!

### What is Amazon Aurora?

Amazon Aurora is a relational database designed for large workloads and high availability due to its cluster-based architecture

### How I used Amazon Aurora in this project

"I used Amazon Aurora as the database backend for my employee management web application. It stored employee information that users could add through the web interface, and I verified the connection using MySQL CLI

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was how much control i have over the EC2 instance from my CLI

### This project took me...

40 min

---

## Creating a Web App

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-webapp_b7999168)

I connected to my EC2 instance using SSH (Secure Shell) from my local terminal with the command ssh -i NextWorkAuroraApp.pem ec2-user@ec2-13-38-26-190.eu-west-3.compute.amazonaws.com. This used my private key file (NextWorkAuroraApp.pem) to securely authenticate and establish a remote connection to the Amazon Linux server running in the EU-West-3 region.

To help me create my web app, I first updated the software on my EC2 instance using this command: sudo dnf update -y. Then I installed these packages: Apache web server, PHP, MariaDB, and php-mysqli for the web app to connect to the database, using this command: sudo dnf install -y httpd php php-mysqli mariadb105. Finally, I started the web server using this command: sudo systemctl start httpd.

---

## Connecting my Web App to Aurora

I set up my EC2 instance's connection details to my database by creating a configuration file called dbinfo.inc in the /var/www/inc directory. This file contains the Aurora database connection details including the database endpoint (writer instance URL), username (admin), password (), and database name (sample) using PHP's define() function. Before creating the file, I had to change the ownership of the /var/www folder from root to ec2-user using sudo chown ec2-user:ec2-user /var/www so I had permission to create and edit files there.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-webapp_1709b25b)

---

## My Web App Upgrade

Next, I enhanced the web app by establishing a database connection, enabling real-time updates and the ability to view database changes through the web interface.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-webapp_2709b25b)

---

## Testing my Web App

I checked if my web app correctly updated the database by:

1- Connecting to the Aurora database using MySQL CLI from my EC2 instance

2- Selecting the 'sample' database with the USE sample command

3- Running SELECT \* FROM EMPLOYEES; to view all records in the EMPLOYEES table

4- Verifying that the data I added through the web app (AyoubYouhad, Maya, and Nail) appeared correctly in the database with their corresponding addresses

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-databases-webapp_1409z22b)

---

---
