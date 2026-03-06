<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Secure Secrets with Secrets Manager

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-secretsmanager)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-security-secretsmanager/architecture-complete.png)

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-secretsmanager_r7s8t9u0)

---

## Introducing Today's Project!

In this project, I will:

🕵️‍♀️ Identify how a web app is insecurely storing credentials.

😮‍💨 See how GitHub Secret Scanning blocks insecure code from being pushed to a repository.

🔐 Update the web app to use AWS Secrets Manager to store and retrieve credentials securely.

👏 Verify that the secured app can be made public without exposing sensitive credentials.

### Tools and concepts

The key services and concepts I learned in this project were AWS Secrets Manager, AWS IAM, and GitHub.

I learned how to securely store credentials using Secrets Manager, manage access with IAM, and avoid exposing secrets in public repositories using GitHub secret scanning. I also learned concepts like hardcoded credentials, secret rotation, and Git rebasing to remove sensitive data from commit history.

### Project reflection

This project took me approximately 60 min

I chose to do this project today because I wanted to learn how to securely manage credentials in applications and avoid exposing sensitive data in code.

Something that would make learning with NextWork even better is more real-world security scenarios and troubleshooting examples to practice fixing common mistakes. 🚀

---

## Hardcoding credentials

Exposing credentials publicly is a major security risk and should never happen in production applications. If someone gains access to these credentials, they could access systems, delete resources, steal sensitive data, or cause serious damage.

've set up the initial configuration using fake credentials (access key ID, secret access key, and region). These credentials are only examples and are used for testing purposes.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-secretsmanager_j2k3l4m5)

---

## Using my own AWS credentials

We installed several packages to build and run the application:

FastAPI – to create the web API and handle requests.

Uvicorn – to run the FastAPI application as a web server.

Boto3 – to allow the app to interact with AWS services, such as listing S3 buckets.

python-multipart – to support handling form data and file uploads in the API.

When I first ran the app, I ran into an error because AWS Access Key ID provided for the web app is not valid.

To resolve the InvalidAccessKeyId error, I created real AWS access keys and updated the config.py file to include them. The file now contains my Access Key ID, Secret Access Key, and AWS region, which the application uses to authenticate and interact with AWS services like S3.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-secretsmanager_wghjteykut)

---

## Pushing Insecure Code to GitHub

Once I updated the web app code with credentials, I forked the repository so I could create my own version of the app and test GitHub’s secret scanning capability.

A fork is different from a clone because a fork creates a copy of the repository under my own GitHub account, while a clone only downloads the repository to my local machine.

To connect my local repository to the forked repository, I set the fork as the remote origin. Then I used git add and git commit to stage and save my changes locally. Finally, git push uploads my commits from the local repository to my forked repository on GitHub.

GitHub blocked my push because the code contained sensitive credentials. This is a good security feature because it prevents accidental exposure of secrets, protecting accounts, data, and resources from unauthorized access.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-secretsmanager_o2p3q4r5)

---

## Secrets Manager

AWS Secrets Manager is a service used to securely store and manage secrets. It provides a centralized place to store, audit, and rotate secrets automatically. I’m using it to store AWS access keys and secret keys.

Other common use cases include storing:

- API keys

- OAuth tokens

- Database credentials

This helps keep sensitive information safe and reduces the risk of accidental exposure in code.

Secret rotation is the process of automatically changing (updating) a secret, like an AWS access key or database password, on a regular schedule. You would use it to reduce the risk of credentials being compromised — even if someone gets access to a secret, it will only be valid for a limited time. It’s especially useful for long-lived applications, shared credentials, or when compliance policies require regular credential updates.

AWS Secrets Manager helps developers integrate with their applications by providing a secure API to store, retrieve, and manage secrets. Instead of hardcoding credentials in the code, the app can call Secrets Manager to get the needed secrets at runtime.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-secretsmanager_h2i3j4k5)

---

## Updating the web app code

I updated the config.py file to retrieve credentials by using the sample code provided by AWS Secrets Manager. The get_secret() function connects to Secrets Manager, requests the secret named "aws-access-key" in the "eu-west-3" region, and returns the secret string. This way, the app no longer needs hardcoded credentials and can securely fetch them at runtime.

I also added code to config.py to extract the AWS access key, secret access key, and region from the Secrets Manager response. This is important because it allows the application to securely retrieve credentials at runtime instead of hardcoding them, reducing the risk of exposing sensitive information.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-secretsmanager_v0w1x2y3)

---

## Rebasing the repository

Git rebasing is an action that rewrites commit history. I used it to remove the commit where I accidentally added AWS credentials. This was necessary because GitHub’s security protection blocked my push due to the exposed credentials in the commit history.

A merge conflict occurred during rebasing because the previous commit contained hardcoded credentials, while the newer commit included code that uses AWS Secrets Manager. I resolved the conflict by keeping the updated Secrets Manager code and removing the old hardcoded credentials.

Once the merge conflict was resolved, I verified that the hardcoded credentials were removed. First, GitHub’s security feature allowed me to push the config file without blocking it. Then, I checked the repository history to confirm that the commit containing the credentials was deleted.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-security-secretsmanager_t5u6v7w8)

---

---
