---
title: "Phase 5 : Deploying Netflix In EKS"
datePublished: Mon Oct 07 2024 08:57:27 GMT+0000 (Coordinated Universal Time)
cuid: cm1ys3ro2000109l5dvsw3d0w
slug: phase-5-deploying-netflix-in-eks
tags: argocd

---

You need cluster and node group to fully operate an EKS environment.

* **Cluster** = Control plane (manages Kubernetes).
    
* **Node group** = Worker nodes (run your apps).
    

## Step 01 : Create Cluster

We are going to create a cluster. In this cluster we will install argo cd, prometheus using helm charts and install grafana.

Here I am going to create cluster in an easy way using eksctl.

### Prerequisites:

1. You need to have one user with admin permissions.
    
2. Choose one micro instance or cloud shell to configure the installtions.
    
3. **aws cli** installed.
    
4. kubectl installed.
    
5. ekctl installed.
    

### Create user and configure

Go to IAM and create user with administration access permission as below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728283327545/ad93217a-2c02-4d59-90a1-0ab905c5171b.png align="center")

Check credentials using

```plaintext
aws sts get-caller-identity
```

### **aws cli installation**

```plaintext
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### kubectl installation

```plaintext
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

### eksctl installation

```plaintext
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

### Errors you may face

1. If you're encountering, `"The maximum number of addresses has been reached"`, means that you have reached the limit on Elastic IP addresses (EIPs) for your AWS account in the region you're working in. By default, AWS limits the number of EIPs per region (typically 5). Since the cluster creation process involves creating a NAT Gateway, which requires an EIP, this limit can block the process.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728283747954/78788114-4188-40e9-9ba9-53c5db57fcb4.png align="center")

In this case you can **Release unused EIPs.** In my case there are , 5 EIP addresses and hence it is not able to create another. Just release unused ones. ( If it is associated with IDs, then delete NAT Gateways first then delete the EIPS. )

2. You may also face roles and permission issues if you are trying to create cluster using Amazon console. It is better to opt using command line.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728284113884/26668ec0-24a2-4af9-a470-0e90a5560579.png align="center")

## Creating cluster

```plaintext
eksctl create cluster --name eks-netflix-1 --version 1.24 --region us-east-1 --nodegroup-name worker-nodes --node-type t2.medium --nodes 1 --nodes-min 1 --nodes-max 1
aws eks update-kubeconfig --region us-east-1 --name eks-netflix-1
kubectl get nodes
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728287706835/d287f502-5706-4bed-93d8-31555968ce47.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728287769555/6a7a0ab2-e05a-477d-b3d9-19120e96917b.png align="center")

Now we are able to run the kubectl commands.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728287835889/734604fc-1677-43f2-ad40-10fb80b9f45e.png align="center")

## Step 04 : Install Argo CD

Go to this site and install argo cd commands.

[https://archive.eksworkshop.com/intermediate/290\_argocd/install/](https://archive.eksworkshop.com/intermediate/290_argocd/install/)

```plaintext
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml
```

You can confirm if argo CD is installed or not using kubectl get ns. You will see argoCD namespace could have been created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728288853724/37f8b253-4362-43dd-9780-10bd6412f86f.png align="center")

```plaintext
kubectl get all -n argocd
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728288923689/8dc9642d-33e9-4a5b-8e99-8050e3e948ee.png align="center")

### Expose argocd server

[https://archive.eksworkshop.com/intermediate/290\_argocd/configure/](https://archive.eksworkshop.com/intermediate/290_argocd/configure/)

```plaintext
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

we created loadbalancer service and it takes bit time to visible in aws.

Once the load balancer is created, we can connect to argocd using DNS, connect to repo and deploy the application.

### Install Helm

```plaintext
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
sudo apt-get update
sudo apt-get install helm
helm version
```

### Install Node Exporter using Helm

To begin monitoring your Kubernetes cluster, you'll install the Prometheus Node Exporter. This component allows you to collect system-level metrics from your cluster nodes. Here are the steps to install the Node Exporter using Helm:

```plaintext
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
kubectl create namespace prometheus-node-exporter
helm install prometheus-node-exporter prometheus-community/prometheus-node-exporter --namespace prometheus-node-exporter
```

After installing prometheus node exporter, verify using

```plaintext
kubectl get ns
kubectl get podds -n prometheus-node-exporter
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728289896956/4f3d546b-2cdc-41dc-9633-84d6996f25e2.png align="center")

Get the DNS endpoint using

```plaintext
export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname'`
echo $ARGOCD_SERVER
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728290053509/24044727-f56b-4ae5-95e8-f33b1df7c4da.png align="center")

Copy paste the endpoint in browser and open via advanced section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728290230994/ed18d940-171c-43de-ab17-dc9c880a2f1c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728290272202/ceeecd36-3fb2-431d-b607-57eee51f69f2.png align="center")

### Login to ArgoCd

Lets now set the argocd password.

```plaintext
export ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
echo $ARGO_PWD
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728290440970/030608f5-7db3-40b8-b46c-01e1af3cbfbb.png align="center")

Use this autogenerated password and login into your argocd.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728290641801/811f28df-67a8-4527-8b92-a9136d399e79.png align="center")

### Connect to Repo in argocd

Step 06 : Deploy App in argo CD

## Step 07 : Setup Monitoring using Helm in the cluster