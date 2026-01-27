<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Connect a GitHub Repo with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-github)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## Connect a GitHub Repo with AWS

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-github_dd9d254e)

---

## Introducing Today's Project!

In this project, I will

🐱 Set up Git and GitHub.

🤝 Connect the web app project to a GitHub repo.

🪄 Make changes to the web app code - and watch  GitHub repo get updated too.

💎 Set up a README file for my repo

### Key tools and concepts

Key Services:

- GitHub - Cloud-based version control repository
- Git - Version control system for tracking code changes
- AWS OAuth - Secure authentication between AWS and GitHub

Key Concepts:

- Version Control - Managing code changes and project history with Git
- GitHub Integration - Connecting GitHub repository to AWS services
- OAuth Authentication - Secure authorization for AWS to access GitHub
- Webhooks - Automated triggers that notify AWS when code changes are pushed
- Remote Development - Configuring Git in cloud environments (EC2)

### Project reflection

This project took me approximately 60 min

I did this project to learn new skills .

This project is part two of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project tomorrow

---

## Git and GitHub

Git is a version control system that tracks changes in files and folders by taking snapshots each time it is updated. I installed Git using the command:

sudo dnf install git -y

GitHub is a platform where developers can store and share their code and projects online. In this project, I used GitHub to track my file changes in a more user-friendly way.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-github_efaadbf7)

---

## My local repository

A Git repository is a folder that contains all project files and their entire version history.

git init is a command that initializes a local Git repository in the current directory.

A branch in Git is a separate line of development that allows you to work on features or changes without affecting the main project. The default branch is usually called master or main. After running git init, the terminal displayed a message confirming that a new Git repository was successfully created.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-github_7bf21bae)

---

## To push local changes to GitHub, I ran three commands

### git add

The first command I ran was git remote add origin, which linked my local repository to the remote GitHub repository. Then, I used git add . to add all project files to the staging area and git commit to save the changes with a message.

A staging area is a place where selected file changes are prepared before being committed to the repository. It allows you to review and organize updates before saving them permanently.

### git commit

The second command I ran was git commit -m, which saved my staged changes to the repository. Using -m means I can add a short message that describes what changes were made in the commit.

### git push

The third command I ran was git push -u origin main, which uploaded my local repository to GitHub. Using -u means it sets the default upstream branch, so future git push and git pull commands can be run without specifying the remote and branch name.

---

## Authentication

When I commit changes to GitHub, Git asks for my credentials to verify that I have the necessary permissions to push changes to the remote origin associated with my local repository.

### Local Git identity

Git needs my name and email because to track who made what change .

Running git logshowed me the history of commits until now, and showed who made each commit.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-github_9a27ee3b)

---

## GitHub tokens

GitHub authentication failed because standard account passwords are no longer accepted for Git operations due to security risks. To securely interact with your repositories and prevent credential interception, you must use a Personal Access Token (PAT) or SSH keys.

A GitHub Personal Access Token (PAT) is a secure, unique string of characters that acts as a digital key for your account, replacing your standard password for command-line operations. You use one because it provides fine-grained security; unlike a password, a token can be restricted to specific tasks (like only reading code or only updating "Gists") and can be revoked instantly if leaked without needing to change your entire account password. GitHub requires them to ensure that even if a connection is intercepted, your primary login credentials remain safe and the scope of potential damage is limited..

To set up a GitHub token, I first go to Developer Settings in my GitHub account to generate a new Personal Access Token with the repo scope. I then copy this token and use it as my password in the terminal when Git prompts for credentials during my next push or pull.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-github_fa11169d)

---

## Making changes again

I wanted to see Git in action, but after editing index.jsp, I couldn't see the changes in my GitHub repository. I realized I hadn't yet committed and pushed the changes.

I wanted to see Git in action, but I didn't see the changes in my GitHub repo after editing index.jsp. I realized I'd forgotten to commit and push them.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-github_6becb2bc)

---

## Setting up a READMe file

A README is an essential document that provides a comprehensive overview of a project. It explains the project's purpose, installation steps, and usage instructions to help users get started quickly.

A README is an essential document that provides a comprehensive overview of a project. It explains the project's purpose, installation steps, and usage instructions to help users get started quickly.

My README documents building a complete CI/CD pipeline using AWS and Java over 7 days.
Key sections:

1- Overview & Architecture - Automated pipeline from code commit to production using EC2, GitHub, CodeArtifact, CodeBuild, S3, CodeDeploy, and CodePipeline.

2-Technologies - AWS services and development tools (Java, Maven, VS Code, GitHub).

3-Day-by-Day Progress - 7 days covering: EC2 setup with VS Code, GitHub integration, dependency management, automated building, deployment, infrastructure as code, and full pipeline orchestration.

4-Setup Guide - Clone repo, install dependencies, configure AWS, deploy pipeline.
Pipeline Flow - Automated workflow from push to deployment.

5-Learnings & Troubleshooting - DevOps principles, best practices, and common issue solutions.

7-Professional portfolio documentation demonstrating production-ready DevOps skills.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-github_c94976902)

---

---
