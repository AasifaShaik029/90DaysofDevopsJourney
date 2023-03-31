---
title: "(2) Part 1-AWS Hands-on: High Availability and Scalability
with ALB | Application Load Balancer|"
datePublished: Fri Mar 31 2023 06:19:05 GMT+0000 (Coordinated Universal Time)
cuid: clfw5mhjc000509mkgsz82mym
slug: 2-part-1-aws-hands-on-high-availability-and-scalability-with-alb-application-load-balancer
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680243489744/20575240-e381-45fd-934e-22dfa4e52b16.jpeg
tags: aws, devops, load-balancing

---

### Scalability

Scalability means that an application or system can handle greater loads by adapting There are two kinds of scalability

1. Vertical scalability
    
2. Horizontal scalability which is also called elasticity
    
    scalability is linked but different to high availability
    
    ### Vertical Scalability (Scale up/down)
    
    Vertical scalability means increasing the size of the instance.
    
    For example, The application runs on t2.micro, vertical scalability here means increasing it to t2.large.
    
    vertical scalability is common for non-distributed systems such as database RDS, and Elastic Cache.
    
    ### Horizontal Scalability (Scale out/in)
    
    Horizontal scalability means increasing the number of instances/systems for your application. Horizontal scaling implies distributed systems this is very common for web applications and model applications.
    
    ### High Availability
    
    High Availability usually goes hand-in-hand with Horizontal Scaling. Horizontal Scaling means running the application **in at least 2 availability zones (AZs)**.
    
    The goal of high availability is to survive a data center loss.
    
    ---
    
    ### Load Balancing
    
    Load balancers are services that forward traffic to multiple servers example ec2 instances downstream.
    
    Ex : If multiple users are trying to access the application then load balancers balance the traffic equally to the Ec2 servers
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680092727195/fab0e11e-b7b8-41a1-a946-0e899737fd6b.png align="center")
    
    ### Advantages of LB
    
    👉 Spread load across multiple downstream instances
    
    👉 Expose a single point of access ( DNS) to the app
    
    👉 Handles failures of downstream instances
    
    👉 Does regular health checks to the instances
    
    👉 High availability across zones
    
    👉 Separate public traffic from private traffic
    
    👉 It is integrated with many other services such as EC2, auto-scaling group, ECS ACM, cloud watch, route 53, AWS WAF, and AWS Global Accelerator.
    
    ### Types of LB
    
    👉 Classic Load Balancer ( Deprecated Now)
    
    👉 Application Load Balancer ( 2016 onwards) --&gt;HTTP, HTTPS, WebSocket
    
    👉 Network Load Balancer (2017 onwards) --&gt; TCP, TLS (Secure TCP), UDP
    
    👉 Gateway Load Balancer (2020 onwards ) --&gt; Operates at 3rd layer - IP Protocol.
    
    ### Load Balancer Security Groups
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680094722568/7ab7d171-461a-4302-baf7-d7149063916b.png align="center")
    
    Allows Users to access our load balancers from anywhere using HTTP/HTTPS. EC2 instances should allow only the traffic which is coming from load balancers.
    
    Here there is an application security group where it will have the security group of the instance. We will link the security group of the ec2 instance with the security group of LB. It will only allow the traffic which is coming from LB.
    
    ### Application Load Balancers
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680242869755/7772239c-da27-4ce9-a7df-94ec45e33393.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680108371609/b542b6d3-51f6-4e05-83b0-a9504f243287.png align="center")
    
    ### Target Groups
    
    A target group is a logical grouping of targets, such as EC2 instances, IP addresses, or AWS Lambda functions, that receive traffic from the ALB.
    
    When you create an ALB, you specify one or more target groups that the ALB routes incoming traffic to. Each target group is associated with a specific port on the ALB, and you can configure health checks to monitor the health of the targets in the group.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680108634068/3cff8528-522c-438c-80ff-0ce64cdb0e92.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680109533361/a869e704-669b-43a1-ad06-b08560fe74fe.png align="center")
    
    There are 2 microservices ( user and search ) that can route their HTTP traffic to 2 target groups by using ALB.
    
    ### Application Load Balancer Hands-On
    
    Let's launch 2 instances that act as a target group. I selected AWS linux AMI.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680110321268/c677c9e6-4370-4cf4-a7be-a342972427fd.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680110388158/6ef35d5b-cac5-41dd-aad8-c988b45978c9.png align="center")
    
    Paste this user data
    
    ```plaintext
    cat install.sh 
    #!/bin/bash
    sudo yum update -y
    sudo yum install -y httpd
    sudo systemctl start httpd
    sudo systemctl enable httpd
    sudo chown -R ec2-user:ec2-user /var/www/html/
    sudo chmod -R 755 /var/www/html/
    #echo"<h1> Hello Aasifa from $(hostname -f)</h1>">/var/www/html/index.html
    echo "<h1> Hello Aasifa from $(hostname -f)</h1>" > /var/www/html/index.html
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680194079661/50452cc3-5466-4a0f-a238-7c84d078d9da.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680194135429/2fe095c5-5d30-4dc8-b082-365acd4e9dcb.png align="center")
    
    You can see that my 2 instances are running but with different url/ip address. Now with load balancing we will run the 2 instances with same URL.
    
    As we are dealing with HTTP traffic, we will use Application Load Balancer.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680196847786/259e8398-ce29-4e8b-ae1a-35ec78b29942.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680196887182/accfd232-5963-4368-8cbd-49ae4b9ba10e.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680196959048/92a55501-9f36-4e06-b4e0-ca6cca7252a9.png align="center")
    
    Create a new security group
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197294536/652728c5-efc3-4dc1-a7e6-36c5d8e40323.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197336111/46541355-14dd-4382-a60e-b4ab546cfa97.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197405832/412249a4-eef6-40cc-8348-4e65a9e0624e.png align="center")
    
    Now we will route our HTTP traffic to target group. Here TG is Our instances created.
    
3. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197484262/628c219e-bea5-435c-8ee7-c9dd47e44e4c.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197621329/918363a4-288a-4c91-8c5c-e27175b746f7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197832971/c6339db8-d130-4f87-ae25-f929fffacdfc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197897527/3c2710f2-4fac-4141-84c5-fb89d7866119.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680197926390/51e9f9a1-a8a5-4cab-aa1e-c5034a2a67ef.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680198131626/19a001be-1d2c-4962-8289-b03a210a92b2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680198195550/b9e1c719-b357-40f1-8bb3-6a5c4fa3eff8.png align="center")

Wait until it becomes active

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680198246775/29516a01-493c-4c96-8812-52af5c90f631.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680198290697/65de2d38-dff6-432d-a67e-9330618125fe.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680198330624/880a14ba-8259-4ae1-aefb-ede6fbaa1bab.png align="center")

When you paste this DNS in the browser then you can see that 2 instances are running with the same URL with balance. When you stop the instance then it will run only the active instances.

\-- 🙂 Thank you for your time. In the next article, we will discuss about Network, Gateway and Elastic Load Balancer 🙂 Follow for more on AWS --