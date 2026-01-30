<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Continuous Integration with CodeBuild

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codebuild-updated)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codebuild-updated_35588a47)

---

## Introducing Today's Project!

In this project, I demonstrate how to build and automate a CI process using AWS CodeBuild. I’m doing this project to strengthen my DevOps skills and gain hands-on experience with real-world workflows.

🛠️ Create and configure a CodeBuild project from scratch
🔗 Connect CodeBuild to a GitHub repository
⚙️ Define the build process using a buildspec.yml file
💎 Automate testing with CodeBuild

### Key tools and concepts

n this project, I learned how to use key AWS services like, CodeBuild, CodeConnections, S3, IAM, and CodeArtifact to build a complete CI/CD pipeline. I learned how to connect GitHub to AWS, automate builds and tests, manage permissions securely with IAM roles, store build artifacts in S3, and download dependencies from CodeArtifact. I also learned the importance of using buildspec.yml and automated testing to make the development process faster, more reliable, and more professional. ✅

### Project reflection

This project took me approximately 60 min

This project is part four of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project tomorrow

---

## Setting up a CodeBuild Project

CodeBuild is a continuous integration service, which means it automatically builds, tests, and packages your code. Engineering teams use it because it simplifies the development process and reduces manual work.

AWS CodeBuild is a fully managed build service. It takes your source code, compiles it, runs tests, and packages it into ready-to-deploy artifacts. Engineers prefer continuous integration tools like CodeBuild because there’s no need to set up or maintain build servers, and you only pay for the compute time used during each build.

My CodeBuild project’s source configuration defines where my application’s code comes from and how CodeBuild accesses it. I selected my GitHub repository as the source so CodeBuild can automatically pull the latest version of my project.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codebuild-updated_fewgrhte)

---

## Connecting CodeBuild with GitHub

There are several ways to connect GitHub to AWS CodeConnections, including GitHub App, personal access tokens, and OAuth apps. I chose the GitHub App because it is the easiest and most secure option, as AWS manages the connection for me and I don’t need to handle tokens or keys manually. This makes it safer and simpler to use for most projects.

The service made it easy to securely link our GitHub repository to AWS without manually handling credentials, allowing CodeBuild and other AWS services to access the code automatically.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codebuild-updated_a7c98e2d)

---

## CodeBuild Configurations

### Environment

My CodeBuild project’s environment configuration means that I set up an on-demand build environment using a managed Amazon Linux image with the Standard runtime and Corretto 8. This allows AWS to create the build resources only when needed, which helps reduce costs.

It includes settings like the provisioning model set to on-demand, the compute type set to EC2, and a managed image provided by AWS. I also created a new service role to securely manage permissions. These settings ensure that my build runs in a reliable, efficient, and properly configured environment. ✅

### Artifacts

The key artifact that this S3 bucket will store is the .war file (Web Application Archive), which is the packaged Java web application containing all the files and resources needed for the server to host the web app.

### Packaging

When setting up CodeBuild, I also chose to package artifacts in a Zip file because it reduces their size for faster uploads and lower storage costs, keeps all files organized in a single package, and makes deployment or sharing much simpler—everything you need is in one neat, easy-to-use file.

### Monitoring

CloudWatch logs are a monitoring service that collects and tracks log data from AWS services. In this project, they record everything that happens during the build process, including the commands that are run, their output, and any errors. This is essential for debugging, tracking build progress, and auditing build activities.

---

## buildspec.yml

My first build failed because CodeBuild didn’t know what commands to run to build my web app. I didn’t have a buildspec.yml file in my repository, so CodeBuild didn’t know what to do.

The first two phases in my buildspec.yml file are install and pre_build. In these phases, I set up the environment and prepare everything needed for the build, such as installing dependencies and checking that all required tools are available.

The third phase in my buildspec.yml file is build. In this phase, I compile my application, run tests, and create the final build files. This is where the main work of building the project happens.

The fourth phase in my buildspec.yml file is post_build. In this phase, I clean up, package the output, and prepare the build artifacts so they can be stored in Amazon S3 or used in the next step of the pipeline.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codebuild-updated_35588a47)

---

## Success!

My second build also failed, but with a different error that said "AccessDeniedException: User: arn:aws:sts::056481036163:assumed-role/codebuild-nextwork-devops-cicd-service-role/AWSCodeBuild-f442b71c-9fe3-4496-9ca1-515b26d65de2 is not authorized to perform: codeartifact:GetAuthorizationToken" CodeBuild was unable to access AWS CodeArtifact to download my project dependencies.

To fix this, I will update the CodeBuild service role and gave it the required permissions to access CodeArtifact. This will allowe CodeBuild to authenticate, download the needed packages, and continue the build process successfully.

The issue happened because the CodeBuild service role didn’t have permission to access CodeArtifact, which is required to retrieve private dependencies during the build.

To resolve the second error, I went to the IAM console and updated the CodeBuild service role by attaching the codeartifact-nextwork-consumer-policy. This gave CodeBuild permission to access CodeArtifact and download the project dependencies it needed.

After adding the required permissions, I returned to the CodeBuild console and retried the build.

To verify the build, I checked my Amazon S3 bucket named nextwork-devops-cicd. I went to the S3 console, opened the bucket, and refreshed the page to see if the build artifact had been uploaded.

After refreshing, I saw the file nextwork-devops-cicd-artifact.zip in the bucket.

Seeing the artifact tells me that my code was successfully compiled and packaged, the build process completed without errors, and the output was correctly stored in S3. It also confirms that my CI pipeline is working properly and that the artifact is ready for the next step, such as deployment. ✅

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codebuild-updated_d9cc6191)

---

## Automating Testing

In a project extension, I added a simple test script to my repository to automatically validate my project structure during the build process.

I added a test script that checks whether the src directory exists and whether the index.jsp file is present in src/main/webapp. These checks help confirm that the basic project setup is correct before the build continues.

If any of these files are missing, the script exits with an error and stops the build. If everything is in place, the script passes and allows the pipeline to move forward. This helps catch basic issues early and keeps the CI process reliable. ✅

To add the test script to the build process, I created a run-tests.sh file with validation tests, updated the buildspec.yml to make it executable with chmod +x and run it with ./run-tests.sh in the build phase, and committed both files to my repository so CodeBuild executes the tests automatically before compilation.

After pushing my code to GitHub, I ran my CI/CD pipeline through AWS  CodeBuild, which automatically triggered the build and test process.

I could see in the CodeBuild logs that my test script was executed during the build phase and that all tests passed successfully. The logs showed messages like “ALL TESTS PASSED,” confirming that the tests were running automatically without any manual intervention.

This verified that my testing process was fully automated and integrated into my CI pipeline. ✅

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codebuild-updated_sm-test-script-upload)

---

---
