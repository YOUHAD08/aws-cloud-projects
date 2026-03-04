<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Deploy Backend with Kubernetes

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks4)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-compute-eks4/architecture-done.png)

---

## Deploy Backend with Kubernetes

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks4_6cfb382f2)

---

## Introducing Today's Project!

In this project, I will:

🚢 Set up the backend of an application for deployment.

⬇️ Install and configure kubectl to manage the cluster.

🚀 Deploy the backend to a Kubernetes cluster.

💎 Monitor and track the deployment using Amazon EKS.

### Tools and concepts

Tools:

eksctl — create the cluster
kubectl — manage Kubernetes resources
Docker — package the Flask app
ECR — store the Docker image
EKS — run managed Kubernetes on AWS

Concepts:

Nodes — EC2 instances forming the cluster
Pods — running instances of your container
Deployments — manage how many pod replicas run
NodePort vs LoadBalancer — two ways to expose your app publicly
Security Groups — AWS firewall to open ports
Pod limits — t2.micro maxes out at 4 pods per node

### Project reflection

This project took me approximately 60 min.

---

## Project Set Up

### Kubernetes cluster

To set up today’s project, I launched a Kubernetes cluster using Amazon EKS. The cluster’s role in this deployment is to run and manage the containerized application across multiple nodes, ensuring scalability, availability, and efficient orchestration of containers.

### Backend code

I retrieved the backend code by cloning the repository:
https://github.com/nextwork-projects/nextwork-flask-backend.git

Pulling the code is essential for this deployment because we need the complete application files — including the Dockerfile — in order to build and package the containerized application.

### Container image

Once I cloned the backend code, I built a container image because Kubernetes deploys applications as containers.

Without a container image, Kubernetes would not be able to package, run, or manage the application consistently across the cluster.

I also pushed the container image to a container registry, which is a centralized repository used to store and manage Docker images.

Amazon ECR facilitates scaling for my deployment because it allows Kubernetes to pull the container image whenever new pods are created, ensuring consistent and reliable application scaling across the cluster.

---

## Manifest files

Kubernetes manifests are sets of instructions written in YAML that tell Kubernetes how to deploy, manage, and run an application inside the cluster.

Manifests are helpful because they allow Kubernetes to automate deployments and maintain the desired state of the application — meaning if something fails, Kubernetes will automatically try to restore it.

A Deployment manifest manages the application by defining how Kubernetes should create, update, and replace groups of containers (pods). It also handles scaling the application and rolling out updates in a controlled way.

The container image URL in my Deployment manifest tells Kubernetes where to pull the container image from so it can deploy the application correctly.

A Service resource handles the networking and routing aspects of a Kubernetes cluster. It defines which port the application listens on, the type of traffic allowed, and the target port for the containers. This allows you to safely expose your application and ensures traffic is correctly routed to the right pods.

My Service manifest sets up:

type: NodePort – Exposes the application on a port of each worker node, making it accessible from outside the cluster.

port: 8080 – The port that the Service exposes internally within the cluster.

targetPort: 8080 – The port on which the container is running.

selector – Links the Service to the correct pods using the label app: nextwork-flask-backend.

This configuration ensures that incoming traffic on port 8080 is routed to the backend application running inside the pods.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks4_b01876554)

---

## Backend Deployment!

To deploy my backend application across the cluster, I applied two Kubernetes manifests:

flask-deployment.yaml – Deploys the application pods and manages scaling, updates, and container resources.

flask-service.yaml – Deploys the Service resource, which handles networking, routing, and exposes the application to external traffic.

### kubectl

kubectl is a command-line tool specifically designed to deploy applications and manage resources within a Kubernetes cluster. I use kubectl to apply manifest files and create the necessary resources (like Deployments and Services) in the cluster.

I can’t use eksctl for this task because it is only meant for setting up and managing the cluster itself, not for deploying applications or managing cluster resources.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks4_6cfb382f2)

---

## Verifying Deployment

My extension for this project was to use the EKS console to view the cluster’s nodes, node groups, and resources. However, I initially couldn’t access the cluster resources because, even though I have administrative access in AWS, Kubernetes (via EKS) has its own access control system.

To gain access, I set up the necessary IAM permissions by running the following command:
"""
eksctl create iamidentitymapping \
 --cluster nextwork-eks-cluster \
 --arn arn:aws:iam::056481036163:user/Ayoub \
 --group system:masters \
 --username admin \
 --region eu-west-3
"""

This mapped my IAM user to the system:masters group in the cluster, giving me admin-level access to manage and view all Kubernetes resources.

Once I gained access to my cluster’s nodes, I discovered the pods running on each node.

Pods are the smallest units of resources in a Kubernetes cluster. They group together one or more related containers to make scaling and management easier. Containers within a pod share the same network namespace and storage, which allows them to communicate easily and access shared data.

The EKS console shows the events for each pod. Here, I could see that the pod was successfully scheduled, pulled the container image, created the container, and started it.

This validated that my backend application was deployed correctly and is running inside the cluster.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks4_3b391f873)

---

---
