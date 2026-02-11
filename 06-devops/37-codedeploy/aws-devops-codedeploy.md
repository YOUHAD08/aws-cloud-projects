<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Deploy a Web App with CodeDeploy

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codedeploy-updated)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codedeploy-updated_val-27)

---

## Introducing Today's Project!

In this project, I will demonstrate how to build an automated deployment system on AWS. The main goal is to strengthen my DevOps skills by practicing real-world deployment and recovery techniques.

Throughout this project, I will:

☁️ Launch a deployment environment using AWS CloudFormation to automate infrastructure setup.

⚙️ Write deployment scripts to automate application deployment commands.

🚀 Deploy a web application using AWS CodeDeploy and verify it in a live environment.

💎 Implement a disaster recovery strategy by rolling back failed deployments.

### Key tools and concepts

Services I used were: CodeDeploy, CodeBuild, CodeArtifact,  CloudFormation, EC2, IAM, S3, and GitHub with CodeConnections.

Key concepts I learnt include: Building end-to-end CI/CD pipelines, infrastructure as code with CloudFormation, deployment strategies and automatic rollbacks, Maven dependency management with private repositories, CodeDeploy lifecycle hooks and appspec.yml configuration, IAM role-based permissions, and AWS resource management and cleanup best practices.

### Project reflection

This project took me approximately 60 min

This project is part five of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project tomorrow

---

## Deployment Environment

To set up for CodeDeploy, I launched an EC2 instance and configured a VPC using CloudFormation. This is important because it provides a secure and controlled environment where my application can run and receive automated deployments.

CloudFormation allowed me to define my infrastructure as code and automatically create the EC2 instance, networking resources, and security settings from a template. This made the setup consistent, repeatable, and easy to manage.

In total, this template creates:

✔️ A complete VPC network
✔️ A public subnet with internet access
✔️ A secured EC2 web server
✔️ IAM permissions for deployment and monitoring
✔️ A public access URL

This allows CodeDeploy to deploy applications automatically to the EC2 instance.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codedeploy-updated_val-5)

---

## Deployment Scripts

Scripts are like mini-programs that automate tasks. They are essentially text files filled with commands that you would normally type one by one in the terminal, but packaged together so they run automatically in sequence.

The install_dependencies.sh script sets up all the software needed to run the website. It installs two programs called Tomcat and Apache. Tomcat is the engine that actually runs the web application in the background, while Apache acts as the front door that visitors talk to first. The script also creates a settings file that tells Apache how to connect to Tomcat, so when someone visits the website Apache quietly passes their request to Tomcat and brings the answer back to the visitor. Together this makes the website accessible to visitors on the internet.

The start_server.sh script starts both Tomcat and Apache on the server and makes sure they will automatically start again if the server ever reboots. Tomcat is the application server that runs the web application in the background, and Apache is the front door that visitors talk to first. By enabling both of them, even if the EC2 instance restarts for any reason, both programs will wake up on their own without anyone having to manually start them again.

The stop_server.sh script safely stops both Apache and Tomcat on the server. Before stopping each one it first checks if they are actually running. This way it only stops them if they are running and does nothing if they are already stopped, which prevents any errors from trying to stop something that is not running.

---

## appspec.yml

Then, I wrote an appspec.yml file to tell CodeDeploy how to deploy the application to the EC2 instance. The appspec.yml file acts like an instruction manual that guides CodeDeploy through each step of the deployment process in the correct order.

The key sections in appspec.yml are the files section which tells CodeDeploy to take the WAR file from the build and place it in the Tomcat webapps folder on the EC2 instance, and the hooks section which runs specific scripts at certain points during the deployment. The hooks run in a specific order — first ApplicationStop runs to stop the old version of the application, then BeforeInstall runs to install the required dependencies like Tomcat and Apache, and finally ApplicationStart runs to start the new version of the application and make the website live again.

I updated the buildspec.yml by adding the deployment files to the artifacts section. Previously the artifacts section only included the WAR file, but now it also includes the appspec.yml file and all the scripts in the scripts folder. I also set discard-paths to no which means the folder structure will be preserved when the artifacts are packaged, so CodeDeploy can find all the files in the correct locations when it deploys the application.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codedeploy-updated_val-12)

---

## Setting Up CodeDeploy

A CodeDeploy application is like the main folder that holds everything related to your project. It is just a container that organizes things and does not do much on its own.
A deployment group is what actually does the work. It is where you decide which specific servers to deploy to, how to deploy, and how to handle failures. Think of it like this — the application is the folder and the deployment group is the plan inside that folder that tells CodeDeploy exactly where and how to deploy.
The best way to understand the difference is that one application can have multiple deployment groups inside it. For example you could have one deployment group for testing, another for staging, and another for production. They all live inside the same application folder but each one targets different servers with different settings. So the application keeps everything organized and the deployment groups do the actual deploying.

CodeDeploy needs IAM roles to get permissions to access and manage AWS resources on my behalf. These permissions let CodeDeploy do things like:

1- Accessing EC2 instances to deploy applications.
2- Reading application artifacts from S3 buckets.
3- Updating Auto Scaling groups.
4- Write CloudWatch logs about what it's doing

Tags help CodeDeploy find the right EC2 instances to deploy to.
Key Benefits:

Auto-discovery - CodeDeploy finds instances tagged with role: webserver and deploys automatically

Scalability - Add new instances with the same tag → they automatically get included in deployments

Clear organization - Tags like role: webserver make it obvious what each instance does.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codedeploy-updated_val-18)

---

## Deployment configurations

CodeDeploy offers three main options:

AllAtOnce deploys to all instances simultaneously - fastest but riskiest since a failure affects everything at once.

OneAtATime updates one instance at a time, verifying success before moving to the next - slowest but safest.

HalfAtATime updates 50% of instances first, then the remaining 50% after verification - balanced between speed and safety.

For production systems, OneAtATime or HalfAtATime reduce risk by limiting the impact of potential failures. For learning environments with a single instance like ours, AllAtOnce makes sense since there's no benefit to the cautious approach.

A small program running on EC2 instance that executes deployments.
How It Works:

1- Listens - Agent constantly checks CodeDeploy for new deployment instructions
2- Downloads - Gets  deployment package (WAR file, scripts, appspec.yml)
3- Executes - Runs the bash scripts and copies files according to appspec.yml
4- Reports back - Tells CodeDeploy if deployment succeeded or failed

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codedeploy-updated_val-20)

---

## Success!

A deployment is a single update to your application. When you create one, you're specifying:

What to deploy - Your application version (WAR file, scripts, appspec.yml)
Where to deploy - Which EC2 instances (via deployment group)
How to deploy - Deployment strategy (AllAtOnce, OneAtATime, etc.)

Each deployment gets a unique ID and is tracked in the deployment history, so you can see what was deployed, when, and whether it succeeded or failed

The revision location is the place where CodeDeploy looks to find the application's build artifacts.

To validate the deployment, I tested the application at http://15.237.118.109/ and confirmed it was accessible and functioning properly.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codedeploy-updated_val-27)

---

## Disaster Recovery

In a project extension, I decided to demonstrate CodeDeploy's rollback capabilities by intentionally breaking a deployment
.
The intentional error I created was misspelling systemctl as systemctll in the stop_server.sh script.

This will cause the deployment to fail because systemctll is not a recognized command, and with set -e enabled in the script, the deployment will immediately fail when it tries to stop the httpd service during the ApplicationStop phase

I also enabled rollbacks with this deployment, which means if the deployment fails, CodeDeploy will automatically revert to the last known working version of the application, restoring service without manual intervention and minimizing downtime.

 In production environments, true automated rollbacks are often implemented with more advanced tools. AWS CodePipeline can be configured to automatically roll back to the last successful deployment when a failure is detected, which eliminates the need for manual intervention.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-codedeploy-updated_rollback-validation-upload)

---

---
