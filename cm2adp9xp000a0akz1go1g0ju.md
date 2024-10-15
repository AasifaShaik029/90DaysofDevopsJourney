---
title: "Complete CICD of Angular App | Gitlab | AWS | ECR | EKS | Sonarqube | Trivy | Black Duck"
datePublished: Tue Oct 15 2024 11:47:30 GMT+0000 (Coordinated Universal Time)
cuid: cm2adp9xp000a0akz1go1g0ju
slug: complete-cicd-of-angular-app-gitlab-aws-ecr-eks-sonarqube-trivy-black-duck
tags: eks, frontend-deployment

---

# Local Deployment

## Gitlab Repo :

[https://gitlab.com/aasifa\_pro/Angular-hotel-app.git](https://gitlab.com/aasifa_pro/Angular-hotel-app.git)

This app was developed by @[NITISH KUMAR VERMA](@nkv) . I would like to express my sincere gratitude to @[NITISH KUMAR VERMA](@nkv) , a highly skilled and talented developer at Verto. His expertise and invaluable support were instrumental in the success of both the project and its deployment. For any development-related assistance, I highly recommend reaching out to @[NITISH KUMAR VERMA](@nkv) —his dedication and knowledge truly set him apart in the field

### Note ( Github to Gitlab )

( If you have any other app which is in github and if you want to use gitlab then you can import using repository url from github to gitlab. )

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728742778919/a99efc5d-17d6-42ce-bc69-d21d157a19f2.png align="center")

# Local Deployment of Angular Hotel App on EC2

I will walk through the steps for deploying the Angular Hotel App on an EC2 instance. I encountered an issue where the app was only accessible via `localhost`, and I’ll also explain how I solved that problem by configuring the IP binding correctly.

## Setting up EC2 for Angular App Deployment

```plaintext
sudo apt update
node -v  # To check the Node.js version
nvm install v20.17.0  # Install a specific Node.js version using NVM
npm install -g @angular/cli
ng version  # To confirm installation
npm install
npm outdated  # Optional: Check for outdated packages
npm update  # Optional: Update outdated packages
npm install
ng build
ng serve --host 0.0.0.0
```

### Issues Faced:

* **Initial Issue**: The application was only accessible locally via `http://localhost:4200`, even when running on an EC2 instance.
    
* **Solution**: The issue was resolved by running `ng serve --host 0.0.0.0`, which exposed the app on the EC2 public IP.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728744491171/aa4e5e45-fa92-4f22-b0e3-8298dfcbcc21.png align="center")

Open your public ip with 4200 to access app.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728744578905/74c6e3b8-af7f-4115-bf21-1aae8d93c707.png align="center")

# Building Dockerfile

```plaintext
#e Node.js as base image
FROM node:20 AS build

# Set the working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy the application code and build the Angular app
COPY . .
RUN npm run build --prod

# Expose port 4200
EXPOSE 4200

# Start the Angular development server
CMD ["npm", "start", "--", "--host", "0.0.0.0", "--port", "4200"]
```

### Note ( 4200 port already exists issue )

The error message indicates that port 4200 is already in use by another process on your EC2 instance. Here’s how to troubleshoot and resolve this:

* **Check Running Containers**: Run the following command to see if any Docker containers are already using port 4200:
    
    ```plaintext
    bashCopy codedocker ps
    ```
    
    If you see any containers using this port, you can stop them with:
    
    ```plaintext
    bashCopy codedocker stop <container_id>
    ```
    
* **Check Other Processes**: If no Docker containers are using it, check if another process is occupying port 4200:
    
    ```plaintext
    bashCopy codesudo lsof -i :4200
    ```
    
    This will show you the process using the port. If it’s not needed, you can kill it using:
    
    ```plaintext
    bashCopy codesudo kill <PID>
    ```
    
    # Steps to Run and Test
    
* ```plaintext
    docker build -t angular-hotel-app .
    docker run -p 4200:4200 angular-hotel-app
    ```
    
    App is running
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728926485940/c7c7a5cb-4cde-4e44-984a-4e469580ad1f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728926469142/02646b32-91e0-43b1-b7e9-bddab4b24448.png align="center")

# Setting up Gitlab in EC2 so that we can push our docker file in gitlab

Please refer [https://mysoftwarediary.hashnode.dev/setting-up-gitlab-in-ec2](https://mysoftwarediary.hashnode.dev/setting-up-gitlab-in-ec2) for the process.

# Integrating GitLab CI/CD Pipeline to Push Docker Images to Amazon ECR

## Install aws-cli

Refer [https://mysoftwarediary.hashnode.dev/aws-cli-installation-and-configuration](https://mysoftwarediary.hashnode.dev/aws-cli-installation-and-configuration)

for installing and configuring aws cli

## **Create ECR Repository**

```plaintext
aws ecr create-repository --repository-name angular-hotel-app --region <your-region>
```

This will create a repository in ECR to store your Docker images.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728930925687/54feedc6-76af-4866-aea8-fd0a26e2464d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728930936780/673b6908-5f2a-4cd1-a61e-9855225ff475.png align="center")

# Update `.gitlab-ci.yml` to Push to ECR

This is to push ro dockerhub ( ignore it )

```plaintext
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_IMAGE: "aasifa/angular-hotel-app:latest"

before_script:
  - echo "Starting GitLab CI/CD Pipeline"

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - echo "Building the Docker image..."
    - docker build -t $DOCKER_IMAGE .
    - echo "Pushing the Docker image to Docker Hub..."
    - docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
    - docker push $DOCKER_IMAGE

# test:
#   stage: test
#   image: node:latest
#   script:
#     - echo "Running tests..."
#     - npm install
#     - npm test

deploy:
  stage: deploy
  image: google/cloud-sdk:latest  # Use the appropriate image for your deployment needs
  script:
    - echo "Deploying to EKS..."
    # Add your EKS deployment commands here, for example:
    #- kubectl apply -f k8s/deployment.yaml
    
```

Lets work on including ECR and EKS in upcoming blog.