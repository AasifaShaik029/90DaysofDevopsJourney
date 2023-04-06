---
title: "(3) Part 2 - AWS Hands-on | Network Load Balancer | Gateway Load Balancer | Elastic Load Balancer |"
datePublished: Thu Apr 06 2023 17:05:47 GMT+0000 (Coordinated Universal Time)
cuid: clg5dd8pa00050ajxfa1jcbsj
slug: 3-part-2-aws-hands-on-network-load-balancer-gateway-load-balancer-elastic-load-balancer
tags: aws, devops

---

### Network LB

The choice between NLBs and ALBs depends on the specific needs of your application and the type of traffic you need to handle.

A Network Load Balancer (NLB) operates at the network layer (Layer 4) of the OSI model.

NLBs are more suitable for handling traffic that requires high performance and low latency, such as TCP and UDP traffic, whereas ALBs are more suitable for applications that require advanced routing, SSL termination, and content-based routing.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680410403984/ec6c653a-c14e-475d-8527-2d02f7bce603.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680411324471/a3d8ee37-c812-4112-944f-edce197ef397.png align="center")

### Hands-On

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680412108414/ee13ccf7-3519-4ffd-bd1e-66348deab178.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680412159447/2096d92c-6ba4-43e3-8d87-da3b0ea6885a.png align="center")

Select all the regions where you want to deploy.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680414057409/0a48832f-1cc6-4c7b-9b2d-f639fcd9fb73.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680414191140/42716a1b-3d77-42b9-ab8f-92b46131dede.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680414259255/d8a6d781-628b-4198-99fd-d77f5cd47fac.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680414341959/f53fbc50-0f8e-41ea-9092-90b9d2f7d98b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680414430567/844aeac4-15ad-45ec-b139-c8869dbdfaad.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680414700625/76a67652-f68a-4f26-b032-0f7bfef7232d.png align="center")

You can see our 2 instances are healthy and by using NLB DNS name we can see if these 2 instances are running

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680415917041/36b963f9-6073-4a81-a4b9-e8541f4d33ac.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680415926234/195ba94a-8315-4d87-bfcf-86a80f370c36.png align="center")

Delete the NLB as it will charge.

### Gateway Load Balancer

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680508512258/6d82ac71-e404-468c-810a-b49f91f5960e.png align="center")

Elastic Load Balancing (ELB) and Application Load Balancing (ALB) are both load-balancing services provided by Amazon Web Services (AWS), but they have some differences.

ELB is a service that automatically distributes incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, within one or more Availability Zones. It can handle both HTTP and HTTPS traffic, and it supports both IPv4 and IPv6. ELB also provides features such as SSL/TLS termination, health checks, and auto-scaling.

ALB, on the other hand, is a more advanced load-balancing service that is designed for modern application architectures, such as microservices and container-based applications. ALB operates at the application layer (layer 7) of the OSI model, whereas ELB operates at the transport layer (layer 4). This means that ALB can intelligently route traffic based on the content of the requests, such as the URL path or the HTTP headers, in addition to factors such as the IP address and port number. ALB also supports advanced features such as content-based routing, WebSocket support, and containerized applications running on Amazon ECS.

In summary, while both ELB and ALB are load-balancing services, ALB is a more advanced and flexible service that is specifically designed for modern application architectures.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680509477182/13ab0d28-7ad5-4ab3-91b2-b2b09f36348d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680510828063/81c6c241-42b9-4a72-a19e-640f1bfda844.png align="center")

---

### Concept of SSL certificates

Generally, client refers to the end-user or the device that is accessing a website or application. The client can be any device that can connect to the internet and access a website, such as a web browser on a desktop or laptop computer, a mobile phone or tablet, or any other internet-connected device.

When a client connects to a website or application that is distributed across multiple servers behind a load balancer, the load balancer acts as an intermediary between the client and the servers. The load balancer receives requests from the client and distributes them across the servers based on various factors, such as the current load on each server or the type of request.

In order to establish a secure connection between the client and the servers behind the load balancer, SSL/TLS encryption is used. This encryption helps to protect sensitive data transmitted between the client and the servers from being intercepted and read by unauthorized third parties. The client must trust the SSL/TLS certificate presented by the load balancer to establish a secure connection.

### SSL Certificates

An SSL (Secure Sockets Layer) certificate is a digital certificate that is used to establish a secure encrypted connection between a web browser and a web server. It is a way to verify the identity of a website and to encrypt the data that is transmitted between the website and the browser.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680511644949/aa695463-0b69-43bd-94cb-fa930e135e95.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680512046446/91029e3f-82ed-4477-af0e-0a5478f50bcb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680512167167/092b52a5-5532-48c1-994b-ea0069874b30.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680752745829/41a34e94-d2cd-4305-ba89-0e18220d6978.png align="center")

### Hands-On

### Enabling Certificates both on ALB and NLB

Got to your Load balancer and Add a listener.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680753492609/5f350900-44d1-45f7-a607-80fd859f4837.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680753525367/8fdda154-07db-4244-bbcb-92fd4f0d83f6.png align="center")

Click on Add. By this we enabled certificate from ACM to the LB. So that client machine can securely connect to the servers.