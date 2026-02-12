<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a CI/CD Pipeline with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codepipeline-updated)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Introducing Today's Project!

In this project, I will  :

1- Create a complete CI/CD pipeline with AWS CodePipeline.

2- Connect the source code, build, and deployment stages.

3- Test automatic deployments and learn how to roll back changes!

### Key tools and concepts

The key services I used were AWS CodePipeline, CodeBuild, CodeDeploy, CodeArtifact, S3, EC2, IAM, and CloudFormation. Key concepts I learned include how to orchestrate a full CI/CD pipeline, execution modes, webhook-triggered deployments, input artifacts, service roles, and how to perform and recover from a deployment rollback.

### Project reflection

This project took me approximately 90 min

---

## Starting a CI/CD Pipeline

AWS CodePipeline is a continuous integration and continuous delivery (CI/CD) service that automates the process of building, testing, and deploying your code.

CodePipeline offers different execution modes  :
1- Superseded – A new execution immediately cancels any in-progress one, ensuring only the latest code is processed.
2- Queued – Executions wait in line and run one at a time.
3- Parallel – Multiple executions run simultaneously and independently.

A service role is automatically created during setup, granting CodePipeline the permissions it needs to access AWS resources like S3 (artifact storage) and CodeBuild (code building).

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_gdnhtm)

---

## CI/CD Stages

The three stages I've set up in my CI/CD pipeline are Source (connected to GitHub to pull the latest code), Build (using CodeBuild to compile and package it), and Deploy (using CodeDeploy to push it to the server). While setting up each stage, I learned about execution modes, service roles, webhook events, input artifacts, and how each stage feeds its output into the next.

CodePipeline organizes the three stages into a visual diagram — Source, Build, and Deploy — each showing a status that updates in real time as the pipeline runs (grey → blue → green, or red if something fails). In each stage, you can see more details on the action provider, execution ID, commit message, and timestamp. As shown in the screenshot, all three stages completed successfully, with every action showing a green checkmark and the commit "fix: Resto" traced across each stage.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Source Stage

In the Source stage, the default branch tells CodePipeline  to monitor this branch for changes and trigger the pipeline whenever there's a commit to it.

The source stage is also where I enable webhook events — automatic notifications sent by GitHub to CodePipeline whenever you push code to the master branch, triggering a new pipeline execution.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_sergt)

---

## Build Stage

The input artifact for the build stage is SourceArtifact
because it's the output of the Source stage — the ZIP file containing our source code — which the Build stage needs as its input to compile and build the application.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_j1k2l3m4)

---

## Deploy Stage

In the Deploy stage, I configured AWS CodeDeploy as the deployment provider to automatically deploy my application to an EC2 instance. I selected the BuildArtifact as the input artifact, chose my existing CodeDeploy application and deployment group, and enabled automatic rollback to ensure the application returns to the last successful version if the deployment fails. 

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_m4n5o6p7)

---

## Success!

Since my CI/CD pipeline gets triggered by any push to the main branch, I tested my pipeline by adding a new line to index.jsp confirming the latest changes were automatically deployed by CodePipeline, then committing and pushing with test: Update index.jsp to validate CodePipeline trigger.

The moment I pushed the code change, CodePipeline automatically detected it via the webhook and triggered a new pipeline execution, moving through Source, Build, and Deploy sequentially. The commit message under each stage reflects the exact commit that triggered the run, making it easy to trace which code change is being built and deployed at any point.

I confirmed the CI/CD pipeline is working by accessing the web app via the EC2 instance's Public IPv4 DNS, where I could see the new line I added to index.jsp live in production — proving that the code change was automatically built and deployed by CodePipeline end-to-end, with no manual steps required.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_e1f2g3h4)

---

## Testing the Pipeline

 I initiated a rollback on the Deploy stage. Automatic rollback is important because if a deployment fails or causes issues, it instantly reverts the application on the server back to the last working version — keeping the source code and build untouched — minimizing downtime and ensuring the app stays stable for users.

The source and build stages are unaffected by the rollback. This is because the rollback only reverts what's deployed on the server — it doesn't touch the source code in GitHub or the build artifacts in S3. To get the rollback working, I also had to update the CodePipeline service role in IAM by attaching the AWSCodeDeployFullAccess policy, as it was missing the permissions needed to perform the rollback.

now gone!7:21 PMAfter the rollback, the live web app reverted to the previous version — the new line I added (<p>If you see this line, that means your latest changes are automatically deployed into production by CodePipeline!</p>) was no longer visible.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codepipeline-updated_sdfgsdfgdf)

---

---
