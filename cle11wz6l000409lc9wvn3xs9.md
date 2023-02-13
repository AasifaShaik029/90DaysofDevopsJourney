# Day 12:  Getting started with Kubernetes

### What is Kubernetes?

K8S is the abbreviation for "Kubernetes", an open-source platform for automating deployment, scaling, and management of containerized applications.

When you look at the logo of Kubernetes, it is actually a ship wheel that is used to control/manage the ship. Here Kubernetes controls/manages the containers.

You can manage all the running containers in one place. Similar to docker swarm.

Multi-container management system. Maintains different services/containers for different functionalities of the application (microservices).

Ex: For Facebook, services should be assigned to authentication, stories, messaging, block lists, etc so that there will be zero downtime.

### Kubernetes Architecture

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675747103313/02773131-9910-4071-9259-79a982e84d83.png align="center")

There will be a **Master and a Node**. Combination of these 2 is called a cluster.

Master Duty: It is to manage the applications running in the node. It ensures that what , how applications should run on a node. It will take care if an application goes wrong.

Node: Applications will be running in the node.

---

Master Side:

You can setup more nodes using master.

Control Panel: You can control the nodes using this control panel.

**API Server:** API server of master allows to connect with the nodes.

**Scheduler:** Used to schedule jobs, containers, tasks to the nodes. Scheduler will contact the API server in the backend and this API server will schedule the jobs to the nodes.

**Kubectl:** API server provides the interface for managing a Kubernetes cluster, while kubectl is a tool used to access and interact with that interface.

Kubectl gives instructions to API server. API server implements the instructions based as given by kubectl.

**etcd:**

etcd is a distributed key-value store(database) that is commonly used as a backing store for configuration management. It stores bac

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675751425408/cee2d1d1-2423-4a74-ab9e-c62f39a0716a.png align="center")

---

**Node Side:**

**Kubelet:** The kubelet communicates with the API server to receive information about what containers should be running on a given node, and then starts and monitors those containers.

**Service Proxy:**

Also known as a kube-proxy, is a component responsible for routing network traffic to the correct pods or services.

---

### pod

It is the smallest and simplest unit in the Kubernetes. It represents a single instance of a running process in your cluster. Pods contain one or more containers, and they provide the environment for these containers to run in.

pod can be a container or group of containers. pod will be running in a node.

---

### Hands-On 😇

We will be using t2 medium as for this hands-on we require at least 2 CPUs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675758785890/4f5e30af-6965-411a-a6b6-1d8580adf8d2.png align="center")

Requirements for Kubernetes

You need docker to be installed if you want to run minikube.

```plaintext
sudo apt-get update
sudo apt-get install docker.io
```

Install minikube (type minikube start ) minikube will install kubernetes for us.

```plaintext

#Install mini kube by following command
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 
sudo install minikube-linux-amd64 /usr/local/bin/minikube

#start it by 
sudo usermod -aG docker $USER && newgrp docker
minikube start --driver=docker
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676015085648/ab5f4dec-4d08-450f-8182-97464e988073.png align="center")

Now minikube is installed. Let's check the containers running. Minikube image is running which contained Kubernetes. **This contains the Kubernetes cluster**.

```plaintext
docker ps
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676015386104/10ad3d38-d759-4b1b-8f95-a6ad13716b81.png align="center")

Let's go inside minikube to see what Kubernetes things persist. It contains a control manager, scheduler,etcd, API server etc

```plaintext
minikube ssh
docker ps
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676015569485/23c08aa1-621b-4476-9222-eaa7283d5aca.png align="center")

### Kubectl

You need to talk to the node from the master. So you need kubectl.

You cant get kubectl by default in this image. You need to install it.

```plaintext
sudo snap install kubectl --classic
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676015847506/90eb1147-cd9b-494d-9c7b-3377d405190c.png align="center")

To install containerized applications, we use snap.

Now we can talk to API server via Kubectl. Now lets communicate with the API server using kubectl.

### Namespace

Asking API server to show how many PODs are there

```plaintext
kubectl get pods
```

Before going into details, lets know about namespace.

🤔Namespace is like a label given to pod.

Example, If we want to store all the pods related to django app then we can create a namespace called django-app-pod and it will contain all django-apps.

---

### Types of Namespaces

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676021079234/5441e8e0-80bb-4e6f-a2bb-42c3fe491765.png align="center")

`default`

Kubernetes includes this namespace so that you can start using your new cluster without first creating a namespace.

`kube-node-lease`

This namespace holds [Lease](https://kubernetes.io/docs/concepts/architecture/leases/) objects associated with each node. Node leases allow the kubelet to send [heartbeats](https://kubernetes.io/docs/concepts/architecture/nodes/#heartbeats) so that the control plane can detect node failure.

`kube-public`

This namespace is readable by *all* clients (including those not authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a requirement.

`kube-system`

The namespace for objects created by the Kubernetes system.

---

To check list of namespaces

```plaintext
kubectl get namespaces
```

This will displays the pods in default namespace.

```plaintext
kubectl get pods
```

This will displays the pods in kubesystem namespace.

```plaintext
kubectl get pods --namespace kubesystem
```

To create a namespace

```plaintext
kubectl create namespace django-todo-app(namespace name)
```

To know no.of nodes

```plaintext
kubectl get nodes
```

---

### Creating a pod in K8S

Now lets create a pod and allocate to namespace. 😉

Note : Inside pod running containers will takes place. No need to build.

To create a pod in kubernetes, you need to run YAML file. 📃

Lets build a small project.

```plaintext
mkdir kubernetes-projects
cd kubernetes-projects
vim pod.yaml
```

We need to create pods in this yaml file.

pod will take the images from dockerhub and runs the container.

```plaintext
kubectl create namespace django-todo-app(namespace name)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676021906616/a5060635-2feb-4a55-8654-f135d300258f.png align="center")

After adding pods in yaml file, create it using

```plaintext
kubectl apply -f pod.yaml
```

pod will be created.

Check if it is running or not using

```plaintext
kubectl get pods --namespace=django-todo-app-ns
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676022112260/89251cdc-17db-4454-9430-8b32823b7d30.png align="center")

check if pod is created

minikube ssh

docker ps

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676022489042/b22ade9e-0fc9-4265-8f29-adda4eb3726f.png align="center")

Note: If you want to update the running container which created using pod, you cannot do that.

deleting pod

```plaintext
 kubectl delete -f pod.yaml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676023293995/e4aaf60b-971a-44f0-9a16-ae5f01ccc6ac.png align="center")

### Deployment file:

Why docker and pod used in production?

If a container/pod crashes we cannot up the container.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676104191821/dfd98771-1754-4c74-b848-8ad175d9faef.png align="center")

We deleted this pod. We cant get back once deleted /crashed. Instead we can use deployment for Auto healing and Auto scaling.

Deployment file is source of truth for one application. Ex: for django pods there will be one application for entire django application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676102476820/06085c38-4ab2-44ef-8b78-ca5ff7666c5c.png align="center")

Here, for each application like djabngo, python, java there will be a separate deployment file. Same deployment file for django related application. If pod is crashed then we can have replicas of nodes which will be mentioned in deployment file. It is similar to namespace where django related apps will be allocated by one namespace but it is having one limitation. **Auto scaling and healing** is not possible.

The label is a name where the application is identified with. Selectors labels should match with this label.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676103910063/7e2e9c77-67da-4abc-bad6-0241d237db14.png align="center")

Here deployment file of django will match with selector having same label.

Replication is done based on lables and selectors.

Writing a deployment file.

kind : deployment --&gt;have features like auto-scaling and auto-healing.

### Hands-On on deployment file 🙂

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676184542665/e7830f46-28ac-4432-bcc3-9fc80b56e2ef.png align="center")

`apiVersion`: Specifies the version of the Kubernetes API that this deployment uses. In this case, it is using version `apps/v1`.

`kind`: Specifies the type of resource that this YAML file is creating. In this case, it is creating a `Deployment`.

`metadata`: Contains information about the deployment, such as its name and labels.

* `name`: Specifies a name for the deployment. In this case, it is named `django-app-deployment`.
    
* `labels`: Associates metadata with the deployment in the form of key-value pairs. In this case, it is using the label `app` with a value of `django-app`.
    
* `namespace`: Specifies the namespace that the deployment belongs to. In this case, it belongs to the `django-todo-app-ns` namespace.
    

`spec`: Specifies the details of the deployment, such as the number of replicas, selector, and template.

* `replicas`: Specifies the number of replicas of the deployment that should be running at any given time. In this case, there should be `3` replicas.
    
* `selector`: Specifies how the deployment should select which pods to manage. The deployment will manage pods that have the labels specified in `matchLabels`. In this case, it will select pods with the label `app` and a value of `django-app`.
    
* `template`: Specifies the properties of the pods that the deployment will create and manage.
    
    * `metadata`: Contains information about the pod, such as its labels. In this case, it is using the label `app` with a value of `django-app`.
        
    * `spec`: Specifies the details of the pod, such as the container and its properties.
        
        * `containers`: Specifies the containers that should run in the pod. In this case, it has only one container.
            
            * `name`: Specifies a name for the container. In this case, it is named `django-todo-ctr`.
                
            * `image`: Specifies the Docker image that should be used to create the container. In this case, it is using the image `aasifa/django-todo-app-img` with tag `latest`.
                
            * `ports`: Specifies the network ports that should be exposed by the container. In this case, it is exposing port `8001`.
                
    
    When we apply the deployment file pods mentioned in the file will be created with mentioned no.of replicas.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676185136790/9ee413d3-aec3-4b4f-ac3c-5e624c76db77.png align="center")
    
    Let's delete (assume crashed) the pods and see if replicas will be created or not.
    
* Here I deleted 3 replicas, but those were recovered by another 3 replicas.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676185653075/c8f83005-359d-4448-ba36-0002e6b8109d.png align="center")
    

It means it auto-healed. This is the advantage of the deployment file.

\--Thats all about k8s basics. 🙂Thank you for your time--