<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launch a Kubernetes Cluster

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks1)

**Author:** siddharthpk nextwork  
**Email:** siddharthpknextwork@gmail.com

---

## Launch a Kubernetes Cluster

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-compute-eks1_e5f6g7h8)

---

## Introducing Today's Project!

In this project, I will launch and manage a Kubernetes cluster on AWS EKS. This involves setting up an EC2 instance, using eksctl to create the cluster, monitoring its creation via CloudFormation, and managing IAM access policies. I'm doing this to gain hands-on experience with Kubernetes on AWS.

### What is Amazon EKS?

### One thing I didn't expect

### This project took me...

---

## What is Kubernetes?

Kubernetes is a container orchestration platform that coordinates containers to run smoothly across servers. It automatically handles tasks like load balancing, scaling, and restarting containers if they fail. People use it because it automates the complex management of hundreds or thousands of containers, allowing developers to focus on building application features instead of manual container oversight.

I used eksctl to create an EKS cluster named nextwork-eks-cluster with a nodegroup named nextwork-nodegroup. The command specified t3.micro EC2 instances as nodes, with an initial count of 3 nodes and automatic scaling between 1 and 3 nodes. It also set the Kubernetes version to 1.33 and targeted a specific AWS region.

I initially ran into two errors while using eksctl. The first one was because eksctl was not installed on my EC2 instance. The second one was because my EC2 instance did not have the necessary IAM permissions to create an EKS cluster, which I resolved by attaching an IAM role with AdministratorAccess.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-compute-eks1_ff9bfc221)

---

## eksctl and CloudFormation

CloudFormation is involved because eksctl uses it under the hood to automate the creation of the EKS cluster. It sets up a CloudFormation stack that provisions all the necessary AWS resources, such as VPC, subnets, and security groups, which would otherwise need to be created manually.

There was a second CloudFormation stack because eksctl separates the core EKS cluster from the node group to manage and troubleshoot each part independently. The cluster is the entire Kubernetes environment, including the control plane, while the node group is a collection of EC2 instances (nodes) that run the containers.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-compute-eks1_w3e4r5t6)

---

## The EKS console

I had to create an IAM access entry because AWS permissions alone do not automatically grant access to Kubernetes resources. An access entry acts as a bridge, mapping my IAM user to Kubernetes' own access control system, allowing me to interact with the cluster's nodes. I set it up by navigating to the EKS console, selecting my cluster, and then creating an access entry for my IAM user.

It took approximately 60 minutes to create my cluster. Since I'll create this cluster again in the next project of this series, this process could potentially be sped up by ensuring all prerequisites are met and potentially using a faster instance type for the control plane if available, though the creation time is largely dependent on AWS provisioning.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-compute-eks1_e5f6g7h8)

---

## EXTRA: Deleting nodes

Did you know you can find your EKS cluster's nodes in Amazon EC2? This is because the nodes in your Kubernetes cluster are actually EC2 instances that run your containers as a single unit. Each node group can contain multiple EC2 instances that share the same settings.

 Desired refers to the number of EC2 instances that your Kubernetes node group aims to maintain at any given time. This is the target number of nodes that should be running to handle your workloads. For example, in your project, when you ran the eksctl create cluster command, you specified --nodes 3, which sets the desired size to 3.



Minimum size is the smallest number of EC2 instances that your node group can scale down to. This ensures that even during periods of low demand, you always have a baseline number of nodes available to keep your cluster operational. In your command, --nodes-min 1 means your node group will always have at least one node.



Maximum size is the largest number of EC2 instances that your node group can scale up to. This prevents your cluster from growing indefinitely and helps manage costs. Your command used --nodes-max 3, setting the upper limit to three nodes.

When I deleted my EC2 instances, Kubernetes automatically detected the loss of these nodes and initiated the process to replace them. This is because Kubernetes is designed for resilience and self-healing, ensuring that your cluster maintains its desired state even when underlying infrastructure components fail.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-compute-eks1_q7r8s9t0)

---

---
