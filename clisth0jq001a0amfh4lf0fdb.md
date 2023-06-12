---
title: "Docker Container Management on AWS: ECS, Fargate and ECR - Part 1"
datePublished: Mon Jun 12 2023 12:14:43 GMT+0000 (Coordinated Universal Time)
cuid: clisth0jq001a0amfh4lf0fdb
slug: docker-container-management-on-aws-ecs-fargate-and-ecr-part-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686571731469/3a69467e-65d9-4a9b-bc60-7a8bb21427a1.png
tags: aws, devops, ecs, aws-fargate

---

Hello Guys !! I hope you are doing great😊

In this article let's learn about Containers on AWS. We heard about these services when working on AWS DevOps. Let's learn about this in this article.

AWS ECS (Elastic Container Service), EC2 (Elastic Compute Cloud), Lambda, and Fargate are all compute services.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686314987857/a2b51b45-d236-4a22-97f3-107b45544017.png align="center")

We know about docker, docker hub, image, containers, and Kubernetes. Let us understand what are the services in AWS that offer the same features as Docker and its components.

### ECR (Elastic Container Registry)

Let's revise what a docker hub is. Docker Hub is a cloud-based registry/repository for Docker images. Similarly, AWS ECR is a fully managed container registry offered by Amazon. It is a proprietary service offered as part of AWS.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686393699865/ad7b9257-12da-4346-8fb4-d4baa597719a.png align="center")

We write a docker file and need to push it to the docker hub or we can also push it to the AWS ECR and pull from ECR and run the image which will create a container.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686314756916/0f33dba7-ca56-4733-a350-e1d15f64b617.png align="center")

### ECS (Elastic Container Service)

ECS manages and orchestrates containers. It allows you to run and manage Docker containers without worrying about the underlying infrastructure. There is one service that plays similar roles as ECS where you can manage containers. That is EC2.

### ECS Vs EC2

Let's know the difference between ECS and EC2. In EC2 we need to install and configure software as per our requirements. You have to take care of managing the servers and the installation process, whereas in ECS, many of those tasks are abstracted away and taken care of by the service itself.

• ECS (Elastic Container Service) utilizes EC2 (Elastic Compute Cloud) instances in the background to run your containers.

• AWS takes care of provisioning and managing the EC2 instances required to run your containers.

• ECS abstracts away the underlying EC2 infrastructure and provides a simplified container management experience, it still relies on EC2 instances to actually execute and host the containers. ECS takes care of provisioning, managing, and scaling the EC2 instances as needed to ensure the smooth operation of your containers

If you use EC2 to manage containers:

* You would typically deploy your application components, such as the frontend, backend, and database, on a single EC2 instance.
    
* All the components would reside on the same server, and you would have to manage the installation, configuration, and scaling of each component manually.
    
* This approach is known as a monolithic architecture, where all components are tightly coupled within a single instance.
    

On the other hand, if you use ECS to manage containers:

* Containers are created separately for each component of your application, such as the front end, back end, and database.
    
* Each component is containerized and can be managed independently, allowing for better isolation and scalability.
    
* You can deploy multiple containers across multiple EC2 instances within an ECS cluster, distributing the workload and resources more efficiently.
    
* This approach is typically based on a microservices architecture, where components are decoupled and can be scaled independently.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686331731132/48e4273d-3551-4e34-a051-428b01f63af5.png align="center")
    
    ### Fargate
    

Fargate provides a serverless experience where the infrastructure management is abstracted away, making it simpler to operate and eliminating the need for manual provisioning and scaling of EC2 instances.

### ECS Vs Fargate

Amazon ECS offers two launch types:

EC2 and AWS Fargate. While both options allow you to run containers.

**EC2 Launch Type:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686386568315/57fb34ba-c44a-4b69-b9d1-882fbea9fc76.png align="center")

With the EC2 launch type in ECS, you have control over the EC2 instances that run your containers. You are responsible for provisioning, configuring, and managing the EC2 instances. You need to choose the appropriate instance types, manage security updates, handle scaling, and monitor the instances yourself. You have more control over the underlying infrastructure, but it requires more operational overhead.

**AWS Fargate Launch Type:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686387063484/565551a8-7d5d-4373-b7df-48d61837c341.png align="center")

Fargate is a serverless compute engine for containers that abstracts away the underlying infrastructure. With Fargate, you no longer need to provision or manage EC2 instances. Instead, you can define and run your containers as tasks directly on Fargate. It automatically handles the underlying infrastructure, including server provisioning, scaling, and management. You only pay for the resources consumed by your containers, without worrying about the EC2 instances.

Fargate offers a higher level of abstraction and removes the need for manual infrastructure management, while the EC2 launch type requires more involvement in managing the underlying EC2 instances.

### Hands-On

Let's deploy node js app using ECS Fargate 🙂

### Create an instance and connect using SSH

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686563272290/27898a90-88f0-4d50-8a02-49459d544431.png align="center")

### Clone the repo

[https://github.com/AasifaShaik029/node-todo-cicd.git](https://github.com/AasifaShaik029/node-todo-cicd.git)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686564020284/5663927f-d3cb-4969-b79e-ee07b9bc408f.png align="center")

### Attach IAM role with the necessary permissions

Create a IAM role which have below permission assigned.

**Note :**

**If we use other servers instead of AWS Instance, we use AWS access keys to connect and do AWS Configure. As we are using AWS service ( instance ) we need to use IAM role to assign permissions.**

```plaintext
AmazonEC2ContainerRegistryFullAccess
AmazonElasticContainerRegistryPublicFullAccess
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686564319060/b49708f9-8a1b-4557-9048-c2bbba3368a6.png align="center")

Now add the IAM to this instance as below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686564669600/01cc263f-45c3-4eac-a2bf-f8d137232800.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686564906903/1da56669-4328-4d44-87eb-716385c38510.png align="center")

So far we set one server with the necessary permissions needed to interact with ECR.

Now by using this instance, we can now push our image to ECR which is like a docker hub.

### Steps to push the image into ECR

1. ### Create public repo
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686565309614/3d21e047-b408-48d8-bf26-89ab49fcf79e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686565433968/1673bc6f-b829-40d7-ba4f-7ecaa1e09a51.png align="center")

### Execute PUSH commands

When you click on 'view push commands', you will see the command used to push the image to ECR

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686565513427/bc616de5-ae37-403a-80d9-87a16f5d30f5.png align="center")

Execute the commands in the ec2 instance

Before that install the below commands to install AWS CLI and Docker

```plaintext
sudo apt update
curl "https://d1vvhvl2y92vvt.cloudfront.net/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
sudo apt  install docker.io -y 
sudo reboot
```

We now have both things installed

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686566414228/19d7c23c-548f-4a4b-be0a-2703ebd532b8.png align="center")

Now execute all the commands to push the image to ECR

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686567219183/558bcb78-a076-4071-802b-041808b61c25.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686567412316/9ecef393-16e3-4b75-9d31-518abdc357d1.png align="center")

Our image is successfully pushed into ECR

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686567506421/f9768cd9-d7f3-41e1-a4c5-0c1f1e1dcb7c.png align="center")

### Create an ECS Cluster

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686567728997/8ee0d3e5-d6e6-4df4-98cf-29afb0444225.png align="center")

Cluster is created

### Create Task definition

To run a Docker container in ECS, you need a task definition

Now create Task definition

paste the image URL from ECR Repo, ports

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686568298341/07a8519b-9d28-442f-8776-a7b822987a1b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686568343918/224ec8cf-4463-4d1f-bd0c-890b0f14a8fd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686568387903/7c13ea30-a7c2-41da-afd0-c7261b91cc2d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686568521519/88784219-cd20-4329-a171-b33093934cb8.png align="center")

### Run the Task

Now lets RUN the task

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686568629746/4ccb37c5-e981-4b2f-add4-efd488476042.png align="center")

Select lunch type as fargate

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686568781603/4bf10879-c2bd-4152-ae89-fb9c8b6d95b3.png align="center")

Add security group by allowing 8000 under networking

Now my task is running

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686568835889/969e03a3-1af6-4ef1-b1b6-fe9b48f1885f.png align="center")

Check if our app is running by using ipaddress from the task

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686569939971/34f1bac2-9018-42c1-93a1-95411d169816.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686569964138/66cd7413-3a39-49d7-8e89-6f43c35c0c1b.png align="center")

🤩🌈Our App is successfully deployed 🤩🌈