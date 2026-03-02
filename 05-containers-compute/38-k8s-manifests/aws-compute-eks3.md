<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Create Kubernetes Manifests

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks3)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-compute-eks3/architecture-done.png)

---

## Create Kubernetes Manifests

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks3_b01876555)

---

## Introducing Today's Project!

In this project, I will:

🛫 Create a Deployment manifest to define how Kubernetes runs and manages the backend application.

🚚 Create a Service manifest to expose the backend so users can access it.

💎 Explore the manifest files in detail to better understand how Kubernetes works behind the scenes.

### Tools and concepts

The key tools and concepts in this project were:

EC2 – to create a platform for managing AWS services.

IAM roles – to grant permissions securely.

eksctl – to easily create and manage an EKS cluster.

Kubernetes – for container orchestration.

Docker – to build container images of the backend.

Amazon ECR – to store and manage container images.

Deployment and Service manifests – to tell Kubernetes how to run and expose the application.

These tools and concepts helped me understand how to deploy, manage, and expose containerized applications in the cloud.

### Project reflection

I chose to do this project today because I wanted to get hands-on experience with Kubernetes, Docker, and AWS, and see how they work together to deploy a real backend application.

Something that would make learning with NextWork even better is having more step-by-step visual guides or diagrams showing how the components—like pods, services, and deployments—connect and interact.

This project took me approximately 60 min

---

## Project Set Up

### Kubernetes cluster

To set up today’s project, I launched a Kubernetes cluster.

First, I created an EC2 instance to act as a platform for managing AWS services. I then created an IAM role with administrative access and attached it to the instance so it had the required permissions.

Next, I installed eksctl to easily manage Amazon EKS clusters.

Finally, I created the Kubernetes cluster using this command:
''''
eksctl create cluster \
 --name nextwork-eks-cluster \
 --nodegroup-name nextwork-nodegroup \
 --node-type t2.micro \
 --nodes 3 \
 --nodes-min 1 \
 --nodes-max 3 \
 --version 1.33 \
 --region eu-west-3
'''

### Backend code

I retrieved the backend I plan to deploy by cloning the nextwork-flask-backend repository from GitHub into my EC2 instance.

### Container image

Once I cloned the backend code, I built a Docker container image using:

docker build -t nextwork-flask-backend .

This command created an image of the application that I can now run and deploy to Kubernetes.

I also pushed the container image to a container registry because Kubernetes needs access to the image in order to deploy the application.

To push the image to Amazon ECR, I first created a repository, authenticated Docker with ECR, tagged the image with the repository URI, and then pushed it using docker push.

---

## Manifest files

Kubernetes manifests are configuration files (usually written in YAML) that define how resources should be created and managed in a Kubernetes cluster.

Manifests are helpful because they let you describe the desired state of your application — and Kubernetes automatically works to maintain that state.

Deployment manifest tells Kubernetes...

A Kubernetes Deployment manages the desired state of your application, including how many replicas of your containers should run and how they are updated.

The container image URL in my Deployment manifest tells Kubernetes which Docker image to use when creating the application pods.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks3_b01876554)

---

## Service Manifest

A Kubernetes Service exposes a set of pods as a network service, allowing external users or other pods to access your application.

You need a Service manifest to define how the application is reachable, including the type of service, ports, and routing rules.

My Service manifest sets up a NodePort service named nextwork-flask-backend that exposes my backend pods on port 8080, allowing external traffic to reach the application running in the Kubernetes cluster.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks3_b01876555)

---

## Deployment Manifest

Annotating the Deployment manifest helped me understand how Kubernetes manages my application because it clarified the purpose of each field, such as replicas, container images, ports, and labels, and how they work together to maintain the desired state.

A notable line in the Deployment manifest is the number of replicas, which specifies how many copies of the application should run simultaneously.

Pods are relevant because each replica corresponds to a pod, and Kubernetes uses them to run and manage the containers that make up the application.

One part of the Deployment manifest I still want to learn more about is how all the components fit together and whether there are other settings or features I should know about, because understanding this fully will help me design more robust and efficient Kubernetes deployments.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks3_6aae73e71)

---
