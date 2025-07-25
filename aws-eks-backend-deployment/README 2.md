<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Set Up Kubernetes Deployment

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks2)

**Author:** Chandu Vinukonda  
**Email:** chanduvinukonda777@gmail.com

---

## Set Up Kubernetes Deployment

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks2_45e6c3de5)

---

## Introducing Today's Project!

In this project, I will pull the app's backend from GitHub and prepare it for deployment on our Kubernetes cluster using AWS because this will teach you how to turn a developer's code into a production-ready, scalable application managed by Kubernetes in a real cloud environment. You will learn to clone the repository, containerize the backend using Docker, push the image to Amazon ECR, and deploy it to your Kubernetes cluster using deployment and service YAML files, ensuring the app runs reliably within your infrastructure. By completing this step, you will understand how to bridge the gap between raw code and live cloud deployments, preparing you for seamless production rollouts in your AWS Kubernetes workflows.

### Tools and concepts

I used Amazon EKS, Git, Docker, and Amazon ECR to deploy the backend application in a scalable and consistent way. Key steps include launching and connecting to an EC2 instance, installing Git to clone the backend code from GitHub, building a Docker container image of the backend, resolving permission errors by configuring Docker group access, creating an ECR repository to securely store the container image, pushing the image to ECR, and preparing Kubernetes to pull and deploy the containerized backend on the EKS cluster.



### Project reflection

This project took me approximately a few hours to complete. The most challenging part was troubleshooting the Docker permission errors and ensuring proper access to the Docker daemon on the EC2 instance. My favourite part was successfully building and pushing the container image to Amazon ECR, knowing that the backend was ready for deployment on Kubernetes and that all the pieces were coming together seamlessly.

Something new that I learnt from this experience was how to seamlessly integrate Docker with Amazon ECR for storing container images and how to configure user permissions on an EC2 instance to allow Docker commands without using sudo. This deepened my understanding of containerization workflows in the AWS ecosystem and prepared me for deploying applications on Kubernetes more efficiently.

---

## What I'm deploying

To set up today's project, I launched a Kubernetes cluster. Steps I took to do this included launching and connecting to an EC2 instance as my admin workstation, installing eksctl and AWS CLI, configuring my AWS credentials on the instance, and using eksctl to create an EKS cluster on AWS. This process ensured I had a fully functional, managed Kubernetes control plane, allowing me to deploy, scale, and manage containerized applications efficiently within a cloud environment, preparing the cluster for deploying the backend app in the next steps of this project.

### I'm deploying an app's backend

Next, I retrieved the backend that I plan to deploy. An app's backend means the server-side part of the application that processes user requests, handles business logic, and manages data storage behind the scenes. I retrieved backend code by installing Git on my EC2 instance and cloning the team member’s GitHub repository containing the backend application. This gave me a local copy of the code to prepare for containerization and deployment on our Kubernetes cluster.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks2_1ebb86c71)

---

## Building a container image

Once I cloned the backend code, my next step is to build a container image of the backend. This is because a container image packages the application code along with all its dependencies and runtime environment, ensuring that the app runs exactly the same way across different machines and environments. Building this image allows Kubernetes to deploy multiple consistent instances of the backend, making the application scalable, portable, and easy to manage in development, testing, and production settings.

When I tried to build a Docker image of the backend, I ran into a permissions error because my current user didn’t have the necessary rights to access the Docker daemon socket (/var/run/docker.sock). This socket requires root or Docker group permissions to communicate with the Docker service, so without the correct privileges, the Docker client couldn’t connect to the daemon to build the image, resulting in a “permission denied” error.

To solve the permissions error, I added my ec2-user to the docker group using the command sudo usermod -aG docker ec2-user and then logged out and logged back in to refresh the group membership. The Docker group is a Linux group that allows non-root users to run Docker commands without needing to type sudo each time, enabling smoother and safer workflow management while maintaining appropriate permissions on the system.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks2_45e6c3de5)

---

## Container Registry

I'm using Amazon ECR in this project to store and manage my backend container image so that Kubernetes can access it securely and reliably when deploying and scaling the application on my EKS cluster. ECR is a good choice for the job because it integrates seamlessly with AWS services, offers private, secure, and highly available storage for container images, and eliminates the need to manage your own container registry infrastructure, making it easier to maintain a consistent, cloud-native deployment workflow.

Container registries like Amazon ECR are great for Kubernetes deployment because they provide a secure, centralized place to store, manage, and version container images, making them easily accessible to your Kubernetes cluster whenever it needs to deploy or scale applications. This ensures consistent, reliable deployments across environments, supports automated vulnerability scanning, and integrates seamlessly with CI/CD pipelines, helping you maintain a clean, cloud-native workflow without manually managing image distribution or storage.

![Image](http://learn.nextwork.org/thoughtful_turquoise_adorable_squirrel/uploads/aws-compute-eks2_l2m3n4o5)

---

## EXTRA: Backend Explained

After reviewing the app's backend code, I've learnt that it is a Python Flask application that serves as the core logic and API for the project, handling user requests and data processing. The backend relies on specific Python dependencies listed in the requirements.txt file, and its environment is containerized using a Dockerfile to ensure consistent deployment. Understanding these components helps me confidently build, deploy, and troubleshoot the app within Kubernetes.

### Unpacking three key backend files

The requirements.txt file lists all the Python packages and dependencies that the backend application needs to run properly. It allows you to install the exact versions of these packages using a package manager like pip, ensuring the environment is consistent across different machines and deployments.

The Dockerfile gives Docker instructions on how to build a container image for the backend application. Key commands in this Dockerfile include specifying the base image (like a Python runtime), copying the application code into the container, installing the required dependencies from requirements.txt, exposing the necessary network ports, and defining the command to run the backend server when the container starts. This ensures the application runs consistently inside its container environment.

The app.py file contains the main Python code for the backend application. It defines the server, routes, and logic that handle incoming requests, process data, and send responses back to users or other services. Essentially, it’s the core of the backend that runs the application’s functionality.

---

---
