<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Set Up a Web App in the Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-vscode)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

## Project Architecture

![Project Architecture](https://learn.nextwork.org/projects/static/aws-devops-vscode/architecture-today.png)

---

## Set Up a Web App Using AWS and VS Code

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_7a1de541)

---

## Introducing Today's Project!

In this project, I will set Up a Web App in the Cloud

This project is part one of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project tomorrow.

I did this project because I needed to learn new skills.

### Key tools and concepts

n this project, I learned how to set up a Java web application in the cloud using AWS EC2, a virtual server. I created an EC2 instance and connected to it securely using SSH with a private key file.

I installed Apache Maven to build Java projects and Java 8 to run the application. Maven helped me quickly generate a web app structure. I also used VS Code's Remote-SSH extension to connect my editor directly to the EC2 instance, making it easy to view and edit files remotely.

I practiced using terminal commands like cd, ls, and pwd to navigate folders, and even edited code using nano. Overall, I gained hands-on experience with cloud computing, remote servers, and managing web applications on AWS.

### Project reflection

This project took me approximately 60 minutes. The most challenging part was configuring the SSH connection between VS Code and my EC2 instance, especially setting the correct file path for my private key and ensuring the permissions were properly configured. It was most rewarding to see my web application files appear in VS Code's file explorer and successfully edit the index.jsp file, knowing I had just built and deployed a working web app entirely in the cloud.

One thing I didn't expect in this project was that I could connect VS Code directly to my EC2 instance and view all the files through a visual interface, in addition to using the command line. This made editing and navigating the web app much easier than working solely in the terminal.

---

## Launching an EC2 instance

### What I did in this step

In this step, I will :

1 - Launch a new EC2 instance.
2 - Set up a key pair for secure access.
3 - Set up network settings for your instance.

I started this project by launching an EC2 instance because i need to build the web app

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_7852fbf3)

### I also enabled SSH

SSH (Secure Shell) is a protocol that allows only authorized users to securely access a remote server. When you connect to an EC2 instance, SSH checks that your private key matches the public key stored on the server.

Once authenticated, SSH creates a secure, encrypted connection between you and the instance. This ensures that all data, including your commands and the server’s responses, is protected, making SSH a reliable and safe way to work with virtual machines. 🔐

### Key pairs

An EC2 key pair is like the keys to your virtual computer. It allows you to securely access your EC2 instance.

It consists of two parts: a public key that AWS stores, and a private key that you download and keep.

When you use the private key to connect, AWS verifies your identity and grants you access to that specific instance, ensuring secure and private login.

### Downloaded key pair file

Once I set up my key pair, AWS automatically downloaded the private key file to my computer, which I use to securely connect to my EC2 instance.

---

## Set up VS Code

### What I did in this step

First, I’ll install VS Code on my computer and set up its built-in terminal to connect to my EC2 instance.

Then, I’ll update the permissions of my key pair file to make sure it can be used securely to log in.

### What is VS Code?

VS Code (Visual Studio Code) is a free, lightweight code editor that helps to write, edit, and manage code with built-in tools like a terminal, debugging, and extensions.

I installed VS Code to build the web app

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_53d05e68)

---

## My first terminal commands

The first command I ran in the terminal for this project was whoami to check the current user.

### Updating file permissions

I also secured my private key by updating its permissions with the command:

chmod 400 nextwork-keypair.pem

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_9328ada1)

---

## SSH connection to EC2 instance

### What I did in this step

In this step, I will Connect to EC2 instance.

### Connecting to EC2

To connect to my EC2 instance, I ran the command
ssh -i nextwork-keypair.pem ec2-user@ec2-35-180-44-185.eu-west-3.compute.amazonaws.com

### This command required an IPv4 address

A server’s IPv4 DNS is its public address that the internet uses to locate and connect to the server.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_e3069dca)

---

## Maven & Java

### What I did in this step

In this step, I will :

1-Install Apache Maven on EC2 instance.
2-Install Amazon Corretto 8, a version of Java.
3-Verify the installations.

### Why I'm using Maven

Apache Maven is a tool that helps developers build, manage, and organize Java projects. It also works as a package manager by automatically downloading and managing the external libraries your project needs.

Maven is required in this project to set up all the necessary web files to create a web app structure

### Why I'm using Java

Java is a programming language used to build different types of applications, from mobile apps to large enterprise systems

Java is required for this project because it is needed to build and run the web application.

---

## Create the Application

### What I did in this step

In this step, I will use Maven commands in the terminal to create and build a Java web application.

### Creating the Java web app

I generated a Java web app using the command

mvn archetype:generate \
 -DgroupId=com.nextwork.app \
 -DartifactId=nextwork-web-project \
 -DarchetypeArtifactId=maven-archetype-webapp \
 -DinteractiveMode=false

### Installing Remote - SSH

I installed Remote - SSH, which is... I installed it to...
VS Code lets you connect directly via SSH to another computer securely over the internet. This lets you use VS Code to work on files or run programs on that server as if you were doing it on your own compute

### SSH configuration details

The configuration file contained the connection details for my EC2 instance, including the host name (public IP/DNS), username, and the path to my private key file, which allowed VS Code to connect securely via SSH.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_2939cf01)

---

## Create the Application

### Exploring the project structure

Based on your VS Code file explorer, you can see the structure of your Java web application:

Main project structure:

- src/main - This is the source code directory where your application code lives
  - resources - Contains configuration files and other resources your app needs
  - webapp - This is where your web application files are stored - WEB-INF folder - Contains important web application configuration files - web.xml - Configuration file that defines how your web app should run - index.jsp - The main page of your web application (this is what users see when they visit your site) - pom.xml - Maven configuration file that manages your project dependencies and build process

This is a standard Maven-based Java web application structure. The index.jsp file is the one mentioned in the tutorial that you'll be editing to customize your web app!

src folder:
This stands for "source" - it's where all your application's source code and files live. Think of it as the main container for everything that makes up your application.

webapp folder:
This stands for "web application" - it contains all the files that make up the actual website/web interface that users interact with. This includes:

- HTML/JSP pages (like index.jsp) - the pages users see
- CSS files for styling
- JavaScript files for interactivity
  -Images and other media
- Configuration files (like web.xml in the WEB-INF folder)

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_45f91fd7)

---

## Using Remote - SSH

### What I did in this step

In this step, I will install a VS Code extension, use it to connect VS Code to my EC2 instance, and explore and edit my Java web application files directly from the editor

### Updating the web app

The index.jsp file is the main page of a Java web application. It is similar to HTML because it displays web content, but it can also include Java code to create dynamic, changing pages based on user input or data, which is why Java is required to run it.

I edited the index.jsp file by saving the new HTML code and refreshing the page using Ctrl + S.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_7a1de541)

---

## Using nano

### Additional improvements

In this secret mission, I will Edit index.jsp using the terminal instead of an IDE.

### Terminal vs IDE

An alternative to using an IDE is to use the built-in Nano text editor in the terminal. To edit index.jsp, I ran the command:

nano index.jsp

Compared to using an IDE, editing index.jsp in the terminal felt more basic and less convenient. I’d be more likely to use an IDE if I needed features like syntax highlighting, code completion, or easier file navigation.

### Verifying my work

To verify my edits in the terminal, I saved the file and checked its contents with nano or reopened it in the editor. It was possible to see my changes in VS Code right away because the editor was connected to the same EC2 instance via SSH.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-devops-vscode_a3324ad41)

---
