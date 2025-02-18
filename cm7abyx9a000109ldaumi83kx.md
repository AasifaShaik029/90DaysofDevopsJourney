---
title: "Get to Know How to Upgrade a Kubernetes Cluster - Part 1"
datePublished: Tue Feb 18 2025 10:17:33 GMT+0000 (Coordinated Universal Time)
cuid: cm7abyx9a000109ldaumi83kx
slug: get-to-know-how-to-upgrade-a-kubernetes-cluster-part-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1739866126234/b6fef1fa-bfd8-41c8-bac2-0ba02e775f26.jpeg
tags: kubernetes-cluster-upgrade

---

Hello DevOps enthusiasts 😃 !

Today Lets learn the importance of knowing kubernetes upgrading process and steps.

Kubernetes comes with new version for every 3 months. We as a DevOps engineers, we should know how to upgrade cluster with zero downtime as this activity is a frequent one.

In an organization, DevOps engineers maintains different clusters like Unit, Test, Stage, Pre, Prod etc.

As users uses prod environment, we should make sure prod is not impacted with our upgradation. Hence we test prior with different lower stage clusters.

Without further delay, lets jump into further steps.

Upgrading cluster correctly is very important to know and if any mistake is done it cant be irreversible.

Here are pre-requisites that you should know before directly upgrading:

# Prerequisites

1. ### Cordon Nodes
    

In **Kubernetes**, **cordoning** a node refers to the action of marking a node as **unschedulable**, meaning no new pods will be scheduled on that node, but the existing pods continue to run without disruptions.

2. ### Understanding **change logs** and **release notes**
    
    It is crucial for ensuring smooth system upgrades and maintaining stability. These documents track updates, detailing new features, bug fixes, and improvements, which helps teams stay informed about changes. For example: If a new change uses new feature, you should not get confused why it is not working with previous one. 🤔
    
3. ### Irreversible
    

For Example : Your current cluster is 1.30 and you upgraded to 1.31. If you want to undo the process, it is not possible as this is irreversible process. You should do fresh start in that case which impacts the product deployment.

Hence it is better to test first in lower environments, and then go with prod.

Start with low level env —&gt; Stage env —&gt; pre prod —&gt; Prod ( Bteer to maintain gap of one week between lower envs to prod to understand smooth upgradation )

4. ### Maintain Same Versions ( Control plane - Nodes - Autoscalers - Kubelet )
    

Control plane and Nodes should be on same versions.

Example:

Control plane ( 1.30 ) Nodes (1.29) —&gt; To upgrade cluster to 1.31 from 1.30 make sure nodes are with 1.30.

If you are using Cluster Auto Scaler ..it also should align with same version before upgrading. Also kubelet should be aligned with same version.

5. ### Five IP addresses available
    

Make sure you have atlease 5 ipaddress available in a subnet you provide in cluster.

### Actual Process of Upgrading an EKS Cluster

We will take EKS as an example to upgrade.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739869982370/77fbbdc6-8ce9-4411-997a-2d4f528a4dc2.jpeg align="center")

1. EKS is a managed Kubernetes service provided by AWS. When we say it is a managed service, it doesnt manage cluster upgrades but manages the following:
    

✍🏻 Availability of Control Plane

✍🏻 Disaster Recovery

✍🏻 Security

✍🏻 Scaling API server

2. Upgrade Node Groups / Nodes / Fargate
    

Has different approaches. ( You will use rolling / force update strategy —&gt; rolling update ( Zero downtime strategy) )

3. Upgrade Add-Ons
    
4. Understand your k8s cluster well
    
5. Test the Upgrade