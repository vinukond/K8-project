<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Deploy Backend with Kubernetes

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks4)

**Author:** Chandu Vinukonda  
**Email:** chanduvinukonda777@gmail.com

---

## Deploy Backend with Kubernetes

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks4_6cfb382f2)

---

## Introducing Today's Project!

In this project, I will secure and scale my Kubernetes deployment on AWS because a production-ready application must be both resilient and protected. After setting up the foundational components in previous projects—like the EKS cluster, container image, and manifest files—the next step is to enhance the cluster’s reliability, performance, and security. This involves configuring autoscaling to ensure the application can adapt to changes in traffic, setting up IAM roles and network policies to manage secure access within the cluster, and implementing a load balancer to distribute incoming traffic effectively. Additionally, I will integrate monitoring and logging tools to gain visibility into the application’s performance and health. These improvements will help ensure that the application is prepared for real-world usage, where scalability and security are critical.

### Tools and concepts

I used Kubernetes, ECR, kubectl, and eksctl to build, deploy, and manage my containerized backend application on AWS. Key concepts include using manifests to define Deployments and Services, containerizing applications with Docker, pushing images to Amazon ECR for versioned storage, and configuring IAM roles and RBAC permissions to securely manage access to the EKS cluster. These tools and concepts enabled scalable, reliable deployment and traffic routing for my app.

### Project reflection

This project took me approximately a few hours to complete. The most challenging part was configuring IAM roles and permissions correctly to ensure smooth access and control over the EKS cluster. My favourite part was seeing my containerized backend successfully deployed and running on Kubernetes, making the whole app accessible and scalable.

---

## Project Set Up

### Kubernetes cluster

To set up today’s project, I launched a Kubernetes cluster. The cluster's role in this deployment is to orchestrate and manage containerized applications by distributing them across multiple nodes, handling scaling, self-healing, and service discovery. I used Amazon EKS (Elastic Kubernetes Service) to provision and manage the Kubernetes control plane, and set up worker nodes using a managed node group. This setup allows me to deploy, monitor, and scale my applications reliably in the cloud.

### Backend code

I retrieved backend code by cloning the GitHub repository that contains the application's backend using Git. Pulling code is essential to this deployment because it gives me access to the source files I need to build a Docker image, which will later be deployed to the Kubernetes cluster on EKS.

### Container image

Once I cloned the backend code, I built a container image because Kubernetes deploys applications by pulling these images from a registry. Without an image, it would be difficult for Kubernetes to replicate and run my application across the cluster. The image contains everything the app needs—code, dependencies, and environment—ensuring consistency no matter where it runs.

I also pushed the container image to a container registry, which is Amazon Elastic Container Registry (ECR). ECR facilitates scaling for my deployment because it provides a highly available, secure, and managed location for storing container images that Kubernetes can easily pull from. By hosting the image in ECR, I ensure that my EKS cluster can reliably access the backend application image whenever it needs to create or scale pods, without depending on local builds or external registries.

---

## Manifest files

Kubernetes manifests are YAML or JSON configuration files that define the desired state of objects within a Kubernetes cluster, such as Deployments, Services, ConfigMaps, and more. Manifests are helpful because they allow developers to declare infrastructure as code, enabling version control, repeatable deployments, and automation. They make it easier to manage complex applications by specifying how resources should behave and interact, all in a consistent and trackable format.

A Deployment manifest manages the creation and lifecycle of pods running containerized applications in a Kubernetes cluster. It ensures that the desired number of replicas (pods) are always running and can automatically replace failed or outdated pods. The container image URL in my Deployment manifest tells Kubernetes which specific image to pull from a container registry (like ECR) and deploy, ensuring the correct version of the backend application is running in the cluster.

A Service resource exposes a set of Pods in Kubernetes, making them accessible to other services or to external users. It acts as a stable networking endpoint, even if the underlying Pods change over time.

My Service manifest sets up a way for traffic to reliably reach my backend application by selecting Pods with specific labels and forwarding requests to them through a defined port. This ensures that users or other parts of the application can communicate with the backend, regardless of which individual Pod is running the code.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks4_b01876554)

---

## Backend Deployment!

To deploy my backend application, I applied the Kubernetes manifests using kubectl apply -f. This included the Deployment manifest, which instructs Kubernetes how to run and manage the backend pods using the container image from ECR, and the Service manifest, which exposes the application and routes traffic to the correct pods. This step ensured my application runs reliably within the EKS cluster and is reachable as intended.

### kubectl

kubectl is the command-line tool used to interact directly with a Kubernetes cluster. I need this tool to apply manifests, check the status of resources like pods and services, and manage deployments within the cluster. I can't use eksctl for the job because eksctl is primarily used for creating and managing EKS clusters, not for deploying or interacting with workloads inside the cluster once it's running.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks4_6cfb382f2)

---

## Verifying Deployment

My extension for this project is to use the EKS console to visually monitor and manage the cluster resources, such as nodes, pods, and services, because the console provides an intuitive interface to check the health and status of my deployments. I had to set up IAM access policies because without proper permissions, I couldn’t view or interact with Kubernetes objects on the cluster through the console or CLI. I set up access by creating IAM identity mappings with eksctl to link my IAM user and roles to Kubernetes RBAC groups, granting me the necessary admin privileges to manage the cluster securely.

Once I gained access into my cluster's nodes, I discovered pods running inside each node. Pods are the smallest deployable units in Kubernetes that encapsulate one or more containers. Containers in a pod share the same network namespace, storage volumes, and can communicate with each other directly, which allows them to work closely together as a single unit within the cluster.

The EKS console shows you the events for each pod, where I could see messages related to the pod’s lifecycle such as scheduling, pulling images, container starts, or any errors like image pull failures. This validated whether my backend pod was successfully created, able to pull the container image from ECR, and running without issues—or if there were problems I needed to fix, like an incorrect image URL or permissions.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks4_3b391f873)

---

---
