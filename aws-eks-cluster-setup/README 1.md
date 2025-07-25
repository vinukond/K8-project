<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launch a Kubernetes Cluster

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks1)

**Author:** Chandu Vinukonda  
**Email:** chanduvinukonda777@gmail.com

---

## Launch a Kubernetes Cluster

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks1_e5f6g7h8)

---

## Introducing Today's Project!

In this project, I will launch a Kubernetes cluster because I am learning how to deploy, manage, and scale containerized applications in a real-world environment using Kubernetes. By doing this, I will understand how to run multiple containers across different nodes, manage resources efficiently, and ensure my Dockerized Nginx web app stays available and resilient even if parts of the system fail. This will help me learn how Kubernetes handles deployments, services, and pods while preparing me for cloud deployments on platforms like AWS, GCP, or Azure in the future.

### What is Amazon EKS?

Amazon EKS (Elastic Kubernetes Service) is a fully managed Kubernetes service that makes it easy to run Kubernetes clusters on AWS without having to manage the control plane yourself. In today’s project, I used Amazon EKS to create and manage a scalable Kubernetes cluster called nextwork-eks-cluster. I set up the cluster, configured node groups with EC2 instances to run my workloads, and tested the cluster’s resilience by observing how it automatically replaced nodes when they were terminated, showcasing EKS’s powerful automation and management features.

### One thing I didn't expect

One thing I didn’t expect in this project was how seamlessly Kubernetes and EKS handle node failures by automatically replacing terminated EC2 instances without any manual intervention. It was impressive to see the cluster self-heal so quickly and maintain its desired state almost like magic

### This project took me...

This project took me about 13 to 15 minutes to complete overall. The part that took the longest was waiting for the CloudFormation stacks to finish deploying the cluster control plane and managed node groups, as this process involves provisioning multiple AWS resources and can take several minutes to complete.


---

## What is Kubernetes?

Kubernetes is an open-source platform that automates the deployment, scaling, and management of containerized applications. Companies and developers use Kubernetes to efficiently run applications across clusters of machines, ensuring high availability, easy scaling, and simplified operations, which helps them deliver software faster and more reliably in complex environments.

I used eksctl to simplify and automate the creation of my Amazon EKS Kubernetes cluster. The create cluster command I ran defined the cluster name, region, node group configuration, and instance types, allowing me to quickly set up a fully managed Kubernetes environment without manually configuring each component.

I initially ran into two errors while using eksctl. The first one was because eksctl wasn’t installed on my system, so I couldn’t run the cluster creation commands. The second one was because my EC2 instance didn’t have the correct IAM role permissions attached, preventing eksctl from accessing the AWS resources needed to create and manage the EKS cluster.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks1_ff9bfc221)

---

## eksctl and CloudFormation

CloudFormation helped create my EKS cluster because it automated the provisioning and management of all the underlying AWS resources needed for the cluster. It created VPC resources because EKS requires a secure and isolated network environment to run Kubernetes nodes and services, so CloudFormation set up the VPC, subnets, security groups, and related networking components to ensure the cluster operates smoothly and securely.

There was also a second CloudFormation stack for provisioning the managed node group (nextwork-nodegroup) that attaches EC2 instances to your EKS cluster for running your workloads. The difference between a cluster and a node group is that the cluster contains the AWS-managed Kubernetes control plane (API server, etcd, and control components) and networking setup, while the node group provisions and manages the EC2 worker nodes (data plane) that actually run your containers and workloads. eksctl uses one stack for the control plane (cluster) and a separate stack for the node group to allow independent updates, scaling, and management of your nodes without modifying the core cluster stack, providing cleaner separation and flexibility in managing your EKS infrastructure.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks1_w3e4r5t6)

---

## The EKS console

I had to create an IAM access entry in order to grant myself the necessary permissions to interact with and manage the EKS cluster (nextwork-eks-cluster) from my local machine or EC2 instance using kubectl and eksctl. An access entry is a defined set of permissions associated with a user or role that allows them to perform actions on AWS resources, in this case on the EKS cluster, including viewing nodes, deploying workloads, and managing configurations. I set it up by attaching the required IAM policies (like AmazonEKSClusterPolicy and AmazonEKSWorkerNodePolicy) to my IAM user or role, and then using the EKS console to add my IAM identity under "Access" for the cluster, ensuring I am recognized as having the necessary administrative rights to interact with the cluster securely.

It took about 13 minutes to create my cluster. Since I'll create this cluster again in the next project of this series, maybe this process could be sped up if I use a larger EC2 instance for provisioning (to reduce throttling during eksctl operations), pre-create and reuse VPC and subnets instead of letting eksctl create them each time, and ensure my IAM and OIDC configurations are set up ahead of time to avoid pauses in the creation workflow.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks1_e5f6g7h8)

---

## EXTRA: Deleting nodes

This is because EKS worker nodes are actually EC2 instances running in your AWS account. When you create a managed node group in EKS, it provisions and manages these EC2 instances behind the scenes to run your Kubernetes workloads, so the nodes appear just like any other EC2 instances in the EC2 console.

Desired size is the number of nodes you want your node group to maintain under normal conditions. Minimum and maximum sizes are helpful for autoscaling—they set the boundaries for how few or how many nodes your cluster can scale down to or up to, ensuring your workloads have enough resources while optimizing cost and performance.

When I deleted my EC2 instances, Kubernetes detected the missing nodes and automatically triggered the creation of new nodes to replace them. This is because Kubernetes constantly monitors the desired state of the cluster and works with the underlying infrastructure (like AWS Auto Scaling groups) to ensure the specified number of nodes is maintained for running workloads reliably.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks1_q7r8s9t0)

---

---
