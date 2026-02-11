<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Infrastructure as Code with CloudFormation

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-cloudformation-updated)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-cloudformation-updated_bd8b836b)

---

## Introducing Today's Project!

In this project, I will :

1- Generate a CloudFormation template.

2-  Fix common errors in infrastructure as code templates.

3-Watch your template generate new resources in your account!

4- Manually add new resources to your template.

### Key tools and concepts

Services: CloudFormation (IaC), IaC Generator, CodeBuild, CodeDeploy, CodeArtifact, EC2, S3, and IAM.
Concepts:

Infrastructure as Code — deploying resources via templates instead of manual clicking
DependsOn — controlling the order resources are created
Circular Dependencies — avoiding resource creation deadlocks
Parameters — making templates reusable and flexible
Stack Management — deploying and rolling back resources as one unit

### Project reflection

This project took me approximately. 60 min

This project is part six of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project tomorrow

---

## Generating a CloudFormation Template

IaC Generator (Infrastructure as Code Generator) is a CloudFormation tool that helps you create templates faster. It scans your AWS account, discovers existing resources, and automatically generates the code for them.

It works in three simple steps:

1️⃣ Scan your AWS account to find resources.
2️⃣ Create a template by selecting the resources you want to manage together.
3️⃣ Import the template into CloudFormation to deploy everything at once.

This makes infrastructure management faster, easier, and more reliable. 🚀

A CloudFormation template is a file (written in YAML or JSON) that defines AWS resources and how they should be created and managed.

The resources I can add to my template include EC2 instances, S3 buckets, IAM roles, VPCs, and many other AWS services.

The resources I couldn’t add to my template were the CodeBuild project and the CodeDeploy deployment group. They require specific configuration details (such as a CodeBuild project’s environment settings) and security permissions that the generator cannot handle automatically.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-cloudformation-updated_0495b046)

---

## Template Testing

Before testing my template, I will remove the existing resources because CloudFormation cannot create resources with duplicate names. This also helps keep my AWS account clean and organized.

When I tested my template, I got an error message related to my IAM policies. It said that AWS couldn’t find the role named codebuild-nextwork-web-build-service-role that CloudFormation was trying to use.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-cloudformation-updated_f56730fd)

---

## DependsOn

The DependsOn attribute tells CloudFormation to wait and create the IAM role first before attaching it to these policies.

I added the DependsOn line to the four IAM Managed Policy resources in my CloudFormation template. These are the policies whose names start with IAMManagedPolicy00policyservicerole..., and I also added it to the CodeArtifact policy so it waits for its related IAM role to be created first.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-cloudformation-updated_f0df8018)

---

## Circular Dependencies

I gave my CloudFormation template another test, but this time I ran into an error known as a “circular dependency.” This happens when two or more resources depend on each other in a way that CloudFormation cannot resolve, creating a loop that prevents the stack from being created.

To fix this error, I searched for IAMRole00codebuild. In the configuration, there’s a section called ManagedPolicyArns that references your IAM policies. These references aren’t needed and are causing the circular dependency error. I deleted all five of those lines to resolve the issue.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-cloudformation-updated_e6fd85ed)

---

## Manual Additions

In a project extension, I manually defined two more resources:

1- CodeBuild Project
2- CodeDeploy Deployment Group ,


I updated my CloudFormation template by adding a CodeBuild project resource. I configured it to pull source code from GitHub using parameterized repository values, defined the buildspec.yml file, and set S3 as the artifact storage location. I also specified the build environment (Amazon Linux 2 with Corretto 8), enabled CloudWatch logs for monitoring, and added a DependsOn attribute to ensure the IAM role and S3 bucket are created before the project.

I introduced parameters to improve reusability and flexibility. Rather than hardcoding values such as GitHub usernames, I parameterized them to make the template more dynamic and customizable.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-cloudformation-updated_1cee0428)

---

## Success!

I could verify all the deployed resources by visiting  Resources tab

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-cloudformation-updated_bd8b836b)

---

---
