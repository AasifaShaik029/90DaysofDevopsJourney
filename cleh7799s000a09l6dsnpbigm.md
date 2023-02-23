# AWS - Basic understanding of its services

Hello Everyone! 😃

This article is all about theory part where I have added the basic knowledge on the AWS services. This article will give basic knowledge on few of the services which will be used in DevOps. Also I have included below repository where I have added my notes on AWS Cloud practitioner. It is useful for the people who want to attempt AWS-Cloud Practioner exam. Lets get started 😊

### Link for AWS services

[AasifaShaik029/AWS-Cloud-Practioner (](https://github.com/AasifaShaik029/AWS-Cloud-Practioner)[github.com](http://github.com)[)](https://github.com/AasifaShaik029/AWS-Cloud-Practioner)

### Cloud Computing

It is the on-demand delivery(you get it when you need it) of compute power, database storage, applications, and other IT resources.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677149623895/f80e903d-bfc8-4ac0-a110-7c883814ef5c.png align="center")

### **Why Cloud computing?**

To maintain your application, all u need is servers, storage, dedicated work, developers, application security.

For this initial setup is very risky and expensive(which needs huge amount of capital to ensure it works properly. If application doesn’t work as expected, money doesn’t come back) and if no. of users increases you have to maintain more servers.

So, this problem can be solved by Cloud computing.

There are cloud providers which satisfy this need.

Example AWS, AZURE, GCP, IBM cloud etc.

---

### IAM - use servies as IAM users

\-Global service

I-identity(authentication)

A-Access(Authorization)

Inorder to access services of aws, you should be a user. As using root user is not a good practice.

**Root user**

whoever created aws account at first. Root user is having complete access to aws services. Once root account is created, then inorder to access its services use users. So that if someone access ur root account. You should pay for [it.So](http://it.So) it’s a best practice to create users.

Root user provies authentication(username and password) but to have  authorization to services , we need access

**Features/types of Identities**

1.IAM User

2.IAM Role—performing actions on ur behalf

Inorder to provide access to ur friend or people in the organization, you need to create either IAM user or IAM Role

**Ex:** For 2 ppl in grp u need to provide 2 accounts

Creation of account to the user-authentication (generates uname and password)

Assigning policies for authorization(to give access to only particular services use policies as permissions)in json format

To provide programmatical access to aws sdk and CLI, ACCESS KEYS and SECRET keys acts as an unamme and password

 Role: It generates temporary credentials which automatically expires after use.

IAM policies(2 types)

1. **AWS Managed**\-(2 types of permissions)
    

1. Full permission (read,write,delete)
    

1. Read only
    

Policy structure:

{

“Version”:2012..

“statement”\[

Effect:”allow”/deny

Action:\* (giving permission to all the servies)

S3:\* (giving all permissions for s3 service)/s3:putobject

Resources: (giving permission for actions arn:bucketname(to one bucket etc))\]}

 **2.Customer Managed**

**Groups:**

For example, in an organization if there are 100 users, it is difficult to give permissions to each user

So groups will be useful

Same type of users can grouped into one grp

And give permissions to that grp

Thus authorization will become easy

After creating user: user should login by  using given user name and generated password.

---

### AWS CLI

We can access AWS CLI in 3 forms.

1. AWS Console
    
2. AWS CLI
    
3. AWS SDK (Software Development Kit)
    

In devops in most of the cases, we will be using AWS CLI to access AWS.

So lets learn about it. You need Access key and secret key to use CLI. Access key is like a user id and secret key is like its password.

Use IAM user as a good practice.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677154944760/cada2992-f362-4b27-9cb0-46dae6cad1f8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677155118188/d5639d10-dc15-4f27-8f42-fa8352b67c41.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677155224218/56753bcf-c28e-462c-87b5-2ad3dbf403fd.png align="center")

Download csv file for the credentials. We got our credentials. So lets jump into CLI.

Before that you want to install AWS cli into ur machine. Check if it is present using /c

```plaintext
aws --version
#configure using , give your credentials here
aws configure
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677155489570/47ddc415-c4db-4a76-ae12-609434c241c1.png align="center")

You can use this CLI to create an EC2 instance, uploading files in a bucket , displaying list of buckets and many more using aws commands.

---

### VPC-Virtual Private Cloud

**Terminologies in VPC**

VPC,Subnets,Internet gatewayIin/out),route tables,security groups.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676977236108/c1eb0635-3c53-4b92-8d22-ac5e75e34d15.png align="center")

Generally, to create a normal Virtual Machine, we need 3 services. EC2(CPU,Ram) EBS(hard disk) VPC(networking)

In aws we have 2 types of services

1\. AWS Managed--&gt;(SAAS services)

2.Customer manager--&gt;(IAAS,PAAS Services)

\--&gt;"Customer Managed servies comes under VPC"

Ex:EC2, Elastic container service,caches,RDS

\--&gt;In these customer managed services, we need to configure VPC

### Understanding of VPC

Take house as an exmaple

\--&gt;MainGate,Main door, Sub doors,living room,Bedrooms,kitchen

\--&gt;Each customer managed service is bound to a network called VPC. --&gt;Network inside a network is called Subnets(2 types)corresponds to AZs --&gt;Public subnet:Any one can access(have internet) --&gt;private:should have access.(Doesnot have internet)

\--&gt;We need to create a gateway to VPC(like maingate to the house) is called Internet Gateway(2 way access) --&gt;Gateways to the subnets are called routers(route tables consists of rules) --&gt;Based on rules of route tables it decides access for public subnets or private subnets

\--&gt;If we want access to the services inside subnets, we must need Internet gatway permission --&gt;To use private subnets , there is a gateway called NAT gateway. --&gt;To use NAT gateway, we need to create in public subnets and thru that u can access private subnets. --&gt;private subnets cannots access public subnets services --&gt;NAT gateway only have Outgoing access not incoming(one way traffic)

\--&gt;To provide security for private subnets, we have, NACL(Network access control lists) that works in subnets levels. (allows an blcokes)(have rules in it) --&gt;SG(security grps)works on server level(only allows rules)

\--&gt;To track or records the logs like who gained access, who denied etc will be stored in VPC File logs.

### VPC Peering:

\--&gt;connect 2 VPC privately using AWS network to behave as they are belongs to same network VPC endpoints: --&gt;To access services privately. ALL the services are interface type but s3 and dynamo db are gateway

---

### KMS

Service used to perform Encryption of service

KMS: (Key management service)--&gt;Encryption for a service --&gt;AWS manages the encryption keys for us.

---

### S3

There are many storage services in AWS. S3, EFS, snow ball, snow edge , Instance store etc.

Amazon S3 (Simple Storage Service) provides object storage, which is built for storing and recovering any amount of information or data from anywhere over the internet.

S3 is a global where it should have unique name. It will store data in the form of objects.

An object consists of data, key (assigned name), and metadata. A bucket is used to store objects. When data is added to a bucket, Amazon S3 creates a unique version ID and allocates it to the object.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677158657148/95a9c5e2-93d3-4588-9850-0c352fa4f0ba.png align="center")

---

### ECS

ECS is like a docker in AWS. It is an open source containerization platform. It enables developers to package applications into containers—standardized executable components combining application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.

### ECS/Fargate/ECR

To run/launch docker containers in AWS --&gt;With ECS you create ec2 insatcnes to run docker containers(not serverless) --&gt;With Fargate will automatically finds a way to run conatainers.

ECR- Its like a dockerhub

Storing docker images--&gt;ECR Running docker images--&gt;ECS/fargate

### Serverless

Developers dont manage server anymore. There will be servers but we dont see them..it will work backend. Developers just need to deploy code. Servers will look over it

The serverless services we used so far are: 1.S3 2.DynamoDB 3.Fargate 4.Lamda

---

### Lambda

(compute service) It manages your application despite of the load.- withour failure. Runs backend code in response to events such as object upload to s3, creating tables in fynamodb et Once you upload your code to lambda, the service handles all the capacity,scaling, pacthing etc and provide admin infra to run your code. By using cloudwatch it will monitor the performa

The code that you run in lambda is called a lambda fucntion.

We upload code in a zip file. Once fucntion is loaded you select the event source to monitor such as amazon s3, dynamodb table. Within few seconds, lambda will be ready to trigger your function automatically.

We have virtual servers in the cloud (EC2 instances) but it is bound to limited memory and cpu. we add Scaling groups where we add/remove servers over time

In lambda , instead of VMs , we use functions(no servers to manage) scaling is automated here have integration with whole services in aws Event driven--&gt;inkoved when needed lambda container image must implement the lambda runtime API

---

\--Lets meet again with AWS projects in the next article 🙂--

\--Thank you for your time--