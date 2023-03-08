---
title: "Project : Reddit Clone App on Kubernetes with Ingress"
datePublished: Wed Mar 08 2023 10:31:16 GMT+0000 (Coordinated Universal Time)
cuid: clezji712001g08mm1ryc971w
slug: project-reddit-clone-app-on-kubernetes-with-ingress
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678271375856/e58e0e61-a6ff-4adb-a86e-f32703a299b0.png
tags: aws, kubernetes, devops, reddit, ingress

---

**Project**

Automating the Delivery of Highly Scalable Reddit Clone Application using CI/CD Pipeline with Docker, Docker Hub, and Kubernetes Minikube.

Key technologies used: Docker, Docker Hub, Kubernetes, Minikube, CI/CD, Containerization, Automation, Monitoring, Logging, Reddit Clone Application.

This project demonstrates how to deploy a Reddit clone app on Kubernetes with Ingress and expose it to the world using Minikube as the cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678097900602/21bf26a4-1ed4-4845-8c02-844f40ef9528.png align="center")

### CI part

1. In your EC2 instance make sure you install docker.
    

```plaintext

# For Docker Installation
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER && newgrp docker
```

1. Clone the below repo in your CI server.
    
    [https://github.com/AasifaShaik029/reddit-clone-k8s-ingress.git](https://github.com/AasifaShaik029/reddit-clone-k8s-ingress.git)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678121923151/0035d849-1700-4773-a20f-ef3940eb20c2.png align="center")
    
2. **Build**
    
    Lets build the docker file using below command.
    
    ```plaintext
    docker build . -t aasifa/reddit-clone
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678181580086/a124a90d-40a9-4a90-bf62-6091716d7048.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678181589197/7d3eec2c-2958-4af0-9b53-f2b6a212849e.png align="center")
    
3. **Push**
    

```plaintext
#to login to dockerhub, give usrname and pwd
docker login
#To view images created
docker images 
#push to dockerhub
docker push aasifa/reddit-clone:latest
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678182035095/c35ee552-6fb8-49fe-a0c9-a24f2bba5b1c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678182078340/d7d3e310-fa7b-4996-b717-768480342744.png align="center")

### Deployment part

Install minikube and kubectl to run kubernetes

```plaintext
# For Minikube & Kubectl
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube 

sudo snap install kubectl --classic
minikube start --driver=docker
```

We need to deploy the image we pushed in dockerhub.

For that we need a deployment file.

**Why deployment file?**

For scalability, if suppose there are so many people who are accessing this application, it may face downtime. So we create replica for the pods so that it can auto heal the application.

**Deployment.yml**

```plaintext
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reddit-clone-deployment
  labels:
    app: reddit-clone
spec:
  replicas: 2
  selector:
    matchLabels:
      app: reddit-clone
  template:
    metadata:
      labels:
        app: reddit-clone
    spec:
      containers:
      - name: reddit-clone
        image: trainwithshubham/reddit-clone
        ports:
        - containerPort: 3000
```

```plaintext
kubectl apply -f Deployment.yml
kubectl get deployment
```

**service.yaml**

To access this application we use service file.

We will be using nodeport service.

```plaintext
apiVersion: v1
kind: Service
metadata:
  name: reddit-clone-service
  labels:
    app: reddit-clone
spec:
  type: NodePort
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 31000
  selector:
    app: reddit-clone
```

The difference between the port 3000 and 31000 is that port 3000 is the port that the service is listening on inside the Kubernetes cluster, while port 31000 is the port that the service is exposed on outside of the Kubernetes cluster, accessible from the internet or other external networks.

```plaintext
kubectl apply -f Service.yml
```

```plaintext
# Access the application using 
 minikube service reddit-clone-service --url
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678257739192/d4917e8d-6432-4eb4-8e0f-73cc828ff7ab.png align="center")

```plaintext
curl -L http://192.168.49.2:31000
```

You can see our app is running in the cluster

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678257834035/c2228989-57eb-4344-8555-56b9600c2936.png align="center")

### Expose the deployment to run in browser

```plaintext
kubectl expose deployment reddit-clone-deployment --type=NodePort
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678260369926/1d936711-a625-420a-83b6-e2447a689941.png align="center")

```plaintext
kubectl port-forward svc/reddit-clone-service 3000:3000 --address 0.0.0.0 &
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678264004890/a53ede8a-5751-4364-9a9c-69128aab1115.png align="center")

Our port id forwarded. Add 3000 to your instance IP address.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678262189681/1d9b85b5-eb42-4fb7-ac5a-0aa44158de34.png align="center")

### Ingress

Enable ingress in minikube

```plaintext
minikube addons enable ingress
```

![]( align="center")

Lets create ingress.yml

```plaintext
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-reddit-app
spec:
  rules:
  - host: "domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
  - host: "*.domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
```

```plaintext
kubectl apply -f ingress.yml
```

```plaintext

curl -L domain.com/test
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678269589639/5c8a401d-cfdc-4cf5-8fae-f5796672ae78.png align="center")

Finally our app is deployed using domain name.

\--Thank you for your time--