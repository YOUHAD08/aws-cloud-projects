<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Set Up Kubernetes Deployment

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks2)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-compute-eks2/architecture-complete.png)

---

## Set Up Kubernetes Deployment

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks2_45e6c3de5)

---

## Introducing Today's Project!

In this project, I will:

📥 Clone a backend application from GitHub.

🐳 Build a Docker image for the backend service.

🗃️ Push the Docker image to an Amazon ECR repository.

🛠️ Troubleshoot installation and configuration issues—sharpening my problem-solving skills along the way.

💎 Explore and understand the backend source code on GitHub.

### Tools and concepts

The key tools and steps in this project were:

- EC2 – to run commands and manage the project.

- Git – to clone the backend code.

- Docker – to build a container image of the backend.

- Amazon ECR – to store the Docker image.

- EKS – to deploy and run the containerized backend.

- Troubleshooting – fixing permissions and build errors along the way.

### Project reflection

This project took me approximately 60 min.

Something new I learned from this project is how to containerize a backend application with Docker and store it in Amazon ECR for deployment on an EKS cluster.

---

## What I'm deploying

To set up today’s project, I launched a Kubernetes cluster.

First, I created an EC2 instance to act as my management server. Then, I installed the eksctl command-line tool to simplify the process of creating and managing the cluster.

Next, I attached an IAM role with Administrator access to the EC2 instance so it could communicate securely with AWS services.

Finally, I created a cluster with three worker nodes using the following command:
"
eksctl create cluster \
 --name nextwork-eks-cluster \
 --nodegroup-name nextwork-nodegroup \
 --node-type t2.micro \
 --nodes 3 \
 --nodes-min 1 \
 --nodes-max 3 \
 --version 1.33 \
 --region eu-west-3
"

This command created an EKS cluster in the eu-west-3 region with a managed node group that can scale between 1 and 3 nodes.

### I'm deploying an app's backend

Next, I retrieved the backend application that I plan to deploy. The backend is the brain and engine of an application—it handles the logic, data processing, and communication behind the scenes.

To retrieve the backend code, I installed Git on my EC2 instance and completed the necessary configuration. Then, I cloned the repository from GitHub to my instance so I could start working with the source code.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks2_1ebb86c71)

---

## Building a container image

Once I cloned the backend code, my next step was to build a container image of the application. Containerizing the app ensures it runs consistently across different environments and eliminates the common “it works on my machine” problem. It also makes it much easier to scale the application up or down as needed.

When I tried to build the Docker image for the backend, I encountered a permissions error. This happened because building Docker images requires elevated (root) privileges, and my user account did not have the necessary permissions to run Docker commands.

To resolve the permissions error, I ran the following command:
"
sudo usermod -a -G docker ec2-user
"
This command adds the ec2-user to the docker group. In Linux systems, members of the Docker group can run Docker commands without needing to use sudo each time. This allowed me to build and manage Docker images without elevated privileges for every command.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks2_45e6c3de5)

---

## Container Registry

I’m using Amazon ECR in this project to securely store Docker images and later pull them using EKS. ECR is an ideal choice because it integrates seamlessly with EKS and provides a centralized repository for managing container images. This makes it easy to update an image in one place and have those updates automatically deployed to multiple containers.

Container registries like Amazon ECR are essential for Kubernetes deployments because they provide a centralized location to store, manage, and pull container images efficiently.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks2_l2m3n4o5)

---

## EXTRA: Backend Explained

After reviewing the app’s backend code, I’ve learned that it fetches data from an external API, processes it, and returns it as a JSON response through a Flask API endpoint.

### Unpacking three key backend files

The requirements.txt file lists ists the dependencies the application needs to run properly.

The Dockerfile provides Docker with step-by-step instructions on how to build the container image. Key commands in this Dockerfile include specifying the base image, copying the application code into the container, installing dependencies, and defining the command to run the application when the container starts.

The app.py file contains the main backend application built with Flask and Flask-RESTx. It defines an API with a single endpoint, /contents/<topic>, which fetches search results from the Hacker News Algolia API based on the provided topic. The code processes the API response, extracts relevant fields (id, title, and url), and returns them as a JSON response.

Additionally, the file sets up CORS headers to allow cross-origin requests and runs the Flask server on all network interfaces (0.0.0.0) at port 8080 in debug mode.

---

---
