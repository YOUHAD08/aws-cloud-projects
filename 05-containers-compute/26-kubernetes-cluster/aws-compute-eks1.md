<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launch a Kubernetes Cluster

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks1)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-compute-eks1/architecture-complete.png)

---

## Launch a Kubernetes Cluster

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks1_e5f6g7h8)

---

## Introducing Today's Project!

In this project, I will:

🚀 Launch and connect to an EC2 instance – get hands-on with a cloud server.

✨ Create a Kubernetes cluster – deploy my very own container orchestration environment.

☁️ Monitor cluster creation with CloudFormation – track resources as they come online.

🔑 Access cluster securely – use an IAM access entry for safe management.

💎 (Secret Mission) – test the resilience of the Kubernetes cluster and see how it handles challenges.

### What is Amazon EKS?

Amazon EKS (Elastic Kubernetes Service) is a managed service that makes it easy to run Kubernetes clusters on AWS without needing to install or operate your own control plane.

### One thing I didn't expect

I didn’t expect how quickly I could spin up a fully functional Kubernetes cluster and deploy applications with minimal manual configuration

### This project took me

This project took me around 60 min to complete from setup to testing.

---

## What is Kubernetes?

Kubernetes is the go-to tool for managing containers across different nodes. Companies and developers use Kubernetes to manage container-based applications because it can handle very complex applications at any scale.

I used eksctl to easily manage my EKS cluster on AWS, as it simplifies the process compared to using the AWS CLI, which is more complex and requires more manual configuration.

I initially ran into two errors while using eksctl.

The first error occurred because I hadn’t installed it on the EC2 instance.

The second error happened because the EC2 instance did not have the required permissions to access EKS and create a cluster.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks1_ff9bfc221)

---

## eksctl and CloudFormation

CloudFormation helped create my EKS cluster because it automatically provisioned all the required infrastructure resources.

Instead of manually creating every single component of the cluster individually, CloudFormation—used behind the scenes by eksctl—automates the process and speeds up the entire setup.

It created the necessary VPC resources, networking components, security groups, and other dependencies required for the EKS cluster to run properly.

The key difference is that a node group represents a set of EC2 instances that run your containers, while the EKS cluster manages the overall Kubernetes control plane.

Separating the EKS cluster stack from the node group stack makes it easier to manage and troubleshoot each component independently, especially if one part encounters issues.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks1_w3e4r5t6)

---

## The EKS console

I had to create an IAM access entry to manage different parts of the cluster.

An access entry is a set of credentials that allows secure access to AWS resources. I set it up by creating an IAM user with the necessary permissions and configuring it with access keys to interact with the EKS cluster.

It took me 20 minutes to create my cluster. Since I’ll be creating this cluster again in the next project of this series, I could speed up the process by saving the CloudFormation templates or using a preconfigured eksctl configuration file to quickly recreate the cluster without manually repeating all the steps.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks1_e5f6g7h8)

---

## EXTRA: Deleting nodes

Basically, in AWS, the resources that run your EKS nodes are EC2 instances. Each node in the Kubernetes cluster is an EC2 instance, so they appear in the EC2 console just like any other virtual machine.

Desired size refers to the number of nodes you want your Kubernetes node group to have by default.

Minimum and maximum sizes are helpful for auto-scaling:

The minimum size ensures your cluster always has enough nodes to run workloads.

The maximum size sets an upper limit so that auto-scaling doesn’t create more nodes than needed, controlling cost and resource usage.

When I deleted the EC2 instances in the node group, new ones were automatically created by Kubernetes. This happens because Kubernetes constantly tries to reconcile the actual state of the cluster with the desired state, ensuring the cluster always has the number of nodes specified in its configuration.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-eks1_q7r8s9t0)

---
