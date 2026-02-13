<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Create S3 Buckets with Terraform

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-terraform1)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

![Image](https://learn.nextwork.org/projects/static/aws-devops-terraform1/architecture-complete.png)

---

## Introducing Today's Project!

In this project, I will :

🛠️ Install and configure Terraform.

🔑 Configure AWS credentials in the terminal.

🪣 Create and manage S3 buckets with Terraform.

💎 Upload files to S3 using Terraform.

### Tools and concepts

In this project, I learned how to use Terraform to provision infrastructure as code and manage AWS resources automatically. I worked with Amazon S3 to create and manage storage buckets, and I configured the AWS CLI to enable programmatic access to my AWS account. I also understood important concepts like providers, state files, access keys, and the Terraform workflow (init, plan, apply).

### Project reflection

This project took me approximately 60 min

I chose to do this project today because I wanted to strengthen my understanding of Infrastructure as Code and gain more hands-on experience with Terraform and AWS. It was a great opportunity to practice provisioning real cloud resources instead of just learning theory.

Something that would make learning with NextWork even better is adding more real-world scenarios and small challenges after each step to test understanding. This would help reinforce the concepts and build more confidence.

---

## Introducing Terraform

Terraform is a IaC tool that helps build and manage the cloud infrastructure using code.

Infrastructure as Code (IaC) is the practice of describing cloud setup (like servers, storage, networks) in plain text files instead of clicking through a web console

main.tf serves as the core file in a Terraform project. It contains the configuration that defines your infrastructure resources. By describing the desired infrastructure state in files like main.tf, Terraform analyzes the changes required and automatically provisions or updates resources to reach that state.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-terraform1_9i0j1k2l)

---

## Configuration files

Terraform stores its instructions in self-contained “blocks.” and there are different kinds of blocks to do different kinds of things

### My main.tf configuration has three blocks

Terraform configuration is defined using blocks inside .tf files like main.tf.

Each block has a specific purpose:

Provider block → Tells Terraform which cloud to use (e.g., AWS).

Resource block → Defines what infrastructure to create (like an S3 bucket).

Other optional blocks include variable, output, data, and module.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-terraform1_ljvh9876)

---

## Customizing my S3 Bucket

For my project extension, I visited the official Terraform documentation to better understand how to configure and customize AWS resources properly.

The documentation shows detailed explanations of providers and resources, including available arguments, required and optional parameters, usage examples, best practices, and code snippets. It also explains how different resources (like aws_s3_bucket) work, how to configure their settings, and how they interact with other AWS services.

For my bucket customization, I added tags to better organize and identify the resource.

Specifically, I included:

tags = {
Name = "My bucket"
Environment = "Dev"
}

This allows me to label the S3 bucket with a name and specify that it belongs to the Development environment.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-terraform1_ffe757cd3)

---

## Terraform commands

Terraform init is the first command you run in a new Terraform project. It prepares your working directory by:

Downloading providers (e.g., AWS) to connect to cloud services

Configuring the backend to store and track infrastructure state

Installing modules used in your configuration

Creating a lock file to ensure consistent provider versions across your team

It basically sets up everything Terraform needs before you start deploying infrastructure.

terraform plan creates an execution plan, showin what changes Terraform will make to theinfrastructure based on the configuration files.

It shows what will be created, updated, or destroyed, so I can review and confirm the configuration before any real changes are made.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-terraform1_3g4h5i6j)

---

## AWS CLI and Access Keys

When I tried to run terraform plan, I received an error message saying: "The security token included in the request is invalid." This happened because Terraform does not have the correct permissions to connect to AWS.

To resolve my error, I first installed the AWS CLI, a tool that allows you to manage AWS services directly from the terminal, eliminating the need to use the AWS Management Console.

I set up access keys to allow the AWS CLI on my local machine to securely access my AWS account. This gives Terraform (and other command-line tools) programmatic access to AWS services, so I don’t have to rely on the AWS Management Console for every action. Access keys are essential for automating infrastructure tasks, like creating S3 buckets or launching EC2 instances.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-terraform1_7j8k9l0m)

---

## Lanching the S3 Bucket

terraform apply executes the changes defined in your Terraform configuration. It creates, updates, or deletes infrastructure resources according to the code you have written.

Terraform commands follow this order: init → plan → apply.

terraform init prepares the project by downloading providers and setting up the state.

terraform plan previews the changes Terraform will make.

terraform apply executes those changes and provisions the infrastructure.

Each command builds on the previous one to safely deploy infrastructure.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-terraform1_1q2w3e4r)

---

## Uploading an S3 Object

I created a new resource block to store a file object in the bucket my_bucket

We need to run terraform apply again whenever we make changes to the Terraform configuration. This ensures that the updated configuration is applied and the infrastructure matches the desired state defined in the code.

To validate that I successfully updated my configuration, I went to check whether the object was created in the bucket. I then downloaded it to verify that it matches the file on my computer.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-terraform1_9o0p1a2s)

---

---
