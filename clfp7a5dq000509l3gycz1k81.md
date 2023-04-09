---
title: "( 1 )-AWS SAA Hands-on: Private and Public IP (IPV4)|Elastci IP"
datePublished: Sun Mar 26 2023 09:31:06 GMT+0000 (Coordinated Universal Time)
cuid: clfp7a5dq000509l3gycz1k81
slug: 1-aws-saa-hands-on-private-and-public-ip-ipv4elastci-ip
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679823014714/9084883f-ac4e-4382-bf06-bfa0d4be4240.jpeg
tags: aws, devops, aws-devops

---

### Public IP Vs Private IP

In AWS, Generally, we use public IP addresses. We cant use private IPs because we are not on the same network. This means if we use private IP which will be within the VPC, we can't access resources such as websites, applications, and databases hosted on AWS.

Note: public IP address will change once the status of the instance is changed.

In that case, we will be using an Elastic IP address.

### Hands-on

Currently, my IP address is as below and it is in a running state.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679821836818/1d0d4f24-d920-4a6c-95e2-f0b63c77d398.png align="center")

I will stop the instance and start again

You can see that my instance restarted and its Public IPV4 address has changed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822094855/f4f34208-44e5-49bb-a462-fdc36e579959.png align="center")

Now let's associate/attach an elastic IP address.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822286628/a8319101-8345-4603-9d64-1f84ebca81d5.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822327106/b418da19-a043-418b-8f82-e3ad951e06af.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822367370/5979edf1-7173-4642-a54e-5c431a587380.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822544985/7af7dec5-c50e-48b7-9a54-2ef64ddac803.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822581380/1efef6b2-d9aa-4b0f-8e33-484cb106af91.png align="center")

We have allocated an Elastic IP address to our instance. Now even if we change our instance state, then the ipv4 address will remain the same.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822646388/4b2176f2-b212-452b-9dc8-547b8d232a67.png align="center")

Note: If we use an elastic IP address it will charge. Better you detach it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822702030/2f7aa8e9-37bc-4866-99fb-d059f277693e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679822752080/598d9486-564b-4fdd-b8db-51cbc0a1e9b8.png align="center")

That's all about Elastic IP address. Hope it helps. Thank you for your time