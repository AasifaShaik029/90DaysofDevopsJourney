---
title: "(4) Auto Scaling Groups"
datePublished: Sun Apr 09 2023 17:02:27 GMT+0000 (Coordinated Universal Time)
cuid: clg9nkimx000109jteef16pue
slug: 4-auto-scaling-groups
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681059738758/80005010-9d67-43d8-a1f4-60e2c21afb7b.png
tags: aws, devops

---

Generally, if you dont use ASG, you will need to manually configure your instances to run in multiple availability zones, and you will need to implement your own mechanisms to replace instances that fail. So it is more difficult to achieve high availability and fault tolerance for your application without ASG.

An Auto Scaling Group (ASG) enables you to scale your EC2 instances based on demand automatically. The main purpose of an ASG is to ensure that you have the right number of instances running to handle the load on your application.

The goal of the auto-scaling group is to

👉Scale-out (add ec2 instances) to match an increased load

👉 Scale in (remove ec2 instances) to match decreased load

👉 and sure we have a minimum and maximum number of ec2 instances running

👉 automatically register new instances to A Load Balancer

👉 Re-create an ec2 instance in case of previous one is unhealthy/terminated

(ASG is free).

Scale-Out - Adding Instances

Scale In - Removing instances

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680848734650/f631e0c7-f910-419d-8c01-fd9bd2462bd5.png align="center")

Minimum Capacity

Minimum capacity is the minimum number of instances that you want to have running **at all times**, even during periods of low demand.

Desired Capacity

Desired capacity is the number of instances that you want to have running **at any given time** to handle the current demand for your application

Maximum Capacity

The maximum capacity should be set based on the limits of your infrastructure, the maximum capacity your application can handle efficiently, and your budget.

If you move your desired capacity to a higher number and if it is less than the maximum capacity, then it will scale out the instances.

Based on Cloud watch alarms, we can scale instances.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680887739758/df2efe3d-9466-4615-8eac-9dd8295b64b1.png align="center")

Lets do Hands-On

Make sure you don't have instances. Because ASG will take care of that.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680949819488/e9266761-220d-4e69-85bb-60da9b7a8a8e.png align="center")

Goto ASG, and create ASG

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680949888372/81d14b52-c491-4fbf-bff9-fab83efffb89.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680950329144/cf69ea19-d228-4518-bef4-365cc3653c20.png align="center")

Create Launch Template

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680950664207/d5df77a7-c995-4eb0-856f-1c3aee3714a8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680950747619/e274549d-2bc0-4cc6-8d33-d62691953792.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680950803892/3c5606f5-fce3-42a7-98c5-4eae6a1a65f3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680951096362/cd32f679-ba2f-4b0b-9ef5-87cb832611f3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680951154136/489e5984-b5e0-48e5-bf3d-b2056b22a4a3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680951284025/d6abbf7b-f557-4c45-be6f-7e8920475dcd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681021210313/de5e0bd8-af73-4104-9597-73738b8fe496.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681021243238/31304132-ba93-41c9-8d34-bf02570f150b.png align="center")

You can see that instances were automatically scaled.

Activity can be seen under activity section

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681033443918/1e401a3b-0a5c-4eb0-b05e-8dd895ccbaf9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681033527896/2f7b79a0-3b1e-4c94-9f51-df8e6d6f8848.png align="center")

Dynamic Scaling Policies

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681034861451/c24b1791-9895-4579-8698-dc62f67e8edb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681035101855/c6549125-33db-47cc-ba57-4afc6a938a82.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681035199503/53644c22-093f-4279-859b-ef7b1a611f7e.png align="center")