# Project - Deploying serverless application using serverless framework

Serverless Architecture-scale your applications in a cost-efficient way(reducing infr coset)

Generally, developers can write and deploy code, while a cloud provider provisions servers to run their applications, databases, and storage systems at any scale.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677221982089/b668c61c-9178-4f5f-ab40-dec98faaff15.webp align="center")

Servers allow users to communicate with an application and access its business logic, but managing servers takes considerable time and resources. Teams have to maintain the server hardware, take care of software and security updates, and create backups in case of failure. By adopting serverless architecture, developers can offload these responsibilities to a third-party provider, enabling them to focus on writing application code.

Let's say I have one to-do application. It will perform write, delete, update, and display operations/functions. To run this application for 1 hour let's say it will take Rs.100. If it runs continuously for 24 hours then it will take Rs.2400. Here we no need to run the application for 24 hours. We just need 1 or 2 hours. But it is getting charged for the rest of the hours as well.

Here comes the serverless architecture where the application only runs whenever we need it and charge for only that period.

In the above diagram, the developer pushes code to the cloud. The function will be defined that triggers the application. Only when it is triggered application will be running.

**Event-Based**

Only when an event triggers, the service will run.

---

### Serverless Services

### Cloud Formation

Infrastructure as a code By cloud formation, you can setup, model, have, provision, and manage all the services in one place so that you can manage to spend less time managing these resources and focus on the runtime of your applications.

### AWS Lambda

This will make your infrastructure robust.

The code that you run in lambda is called a lambda fucntion.

In lambda , instead of VMs , we use functions(no servers to manage) scaling is automated here have integration with whole services in aws Event driven--&gt;inkoved when needed lambda container image must implement the lambda runtime API.

### AWS API Gateway

AWS lambda will be integrated with AWS API Gateway. Handles scalability. Creates endpoint of the application.

---

To run serverless app you need a server. 😬

Just to make application we will be using EC2( not serverless)

### Project

A serverless framework is a popular tool for building and deploying serverless applications.

Let's install a serverless framework on our machine ( I am using ubuntu installed on my machine. You can use an EC2 instance as well)

Serverless framework is built in node so install node first.

👉 [How To Install Node.js 14 on Ubuntu 22.04|20.04|18.04 | ComputingForGeeks](https://computingforgeeks.com/install-node-js-14-on-ubuntu-debian-linux/#:~:text=Install%20Node.js%2014%20on%20Ubuntu%2022.04%7C20.04%7C18.04%201%20Step,...%203%20Step%203%3A%20Install%20Node.js%20Dev%20Tools)

```plaintext
    sudo apt update
    curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
    sudo apt -y install nodejs
    node  -v
```

**Steps to install serverless**

👉 [Setting Up Serverless Framework With AWS](https://www.serverless.com/framework/docs/getting-started)

```plaintext
sudo npm install -g serverless
```

👉 Now let's create a directory where we will be building our serverless project.

We can make different types of serverless projects as shown below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677393199593/6cfe94b6-764f-49f4-98c4-bd02cdacb588.png align="center")

👉 We are going to make an HTTP API of Node js.

When we give hello in API-endpoint, it should give hello world.

```plaintext
choose AWS - Node.js - HTTP API
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677402840125/e58d3bd2-be10-4fcb-b2f7-751fe0fdaa2c.png align="center")

Our project directory will be created where you can find the below files already persists.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677403077591/2cec1e4f-dc8f-497e-a554-826da60b3384.png align="center")

index.js: API will be written inside this file. It means it will receive the request and passes the response.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677404116072/919e48e5-6ff9-47f5-86d3-5ddde843846b.png align="center")

👉 This is a javascript code that uses the AWS lambda function.

👉 module.exports.handler = async (event) = module.exports.handler is the entry point of the Lambda function. event is the input parameter that is passed and invokes the lambda function.

👉 return returns the response. statusCode is an HTTP status code. 200 is the code which means the function executed successfully.

👉 It gives the success message if the function is executed successfully and gives the input data.

👉This Lambda function returns a successful response with a message and the input data passed to it.

serverless.yml:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677405936815/5411ea62-dce7-4f7f-b6b8-31194ab6fa0b.png align="center")

This is a configuration file.

👉This serverless.yml file defines a service called first-serverless-hello-api and specifies that the framework version used is 3

👉 Cloud provider we are using is AWS. The application will use the Node.js version 18.x runtime on AWS Lambda.

As we are using AWS, we need to connect to the AWS account using IAM User Access Keys.

**Connecting serverless with the access keys of a user :**

Give admin access as of now not to miss any services.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677412814909/7d236379-9a07-437b-95af-6d9397ee2b5c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677412972770/44be9d6e-7285-4456-87f1-c21ecdbe4d29.png align="center")

Make sure you installed was cli in your machine. Add these credentials in your machine using below command.

```plaintext
aws configure
```

Now we connected with our was account. If we deploy our application then it will deploy into our connected was account.

Automatically it will create lambda function, API endpoint.

I added region in our serverless.yml file so that in this region our services will be created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677413617202/de102734-95ca-47bf-9a6d-73afef9a963c.png align="center")

Now lambda function should be triggered. We need one endpoint URL. If we click that URL, then the function will be triggered.

For that API Gateway comes into picture.

Let's talk about API Gateway little bit 🙂

It has 4 functions.

GET: you are getting some response

PUT: update

POST: Insert

DELETE: delete

To access all these functions you need an end-point.

Endpoint is like you have a URL and function attached to it.

EX: https://aasifa.com/viewblog

here https://aasifa.com becomes URL and the endpoint is viewblog. If we hit that endpoint function will be triggered and you get response.

Let's deploy the application now.

Before that login to your serverless dashboard as below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677415576650/57082f33-443e-46b4-80b9-3ed5da3c745b.png align="center")

Now deploy the application using the server.

I got an error saying the signature expired.

try

```plaintext
sudo apt install ntp
sudo apt install ntpdate
sudo ntpdate ntp.ubuntu.com
```

serverless deploy now.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677416291487/2f701865-a7f6-4c65-a571-9081740d58d0.png align="center")

Our app ran successfully

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677416355570/222dd0f3-8286-4692-9151-a1d7035b6bd7.png align="center")

I removed input data from index.js file and deployed again

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677416555858/29a947f7-0ff0-4fb5-bf61-36cc2b6289c4.png align="center")

To deploy in our serverless app we can use

```plaintext
serverless --org=aasifa
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677416837487/e62d8ff7-b2e7-47ed-a908-7aa84406a700.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677416856129/759ed033-8cf7-46db-86cb-6ae54915e978.png align="center")

I added path in serverless.yml file as endpoint

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677417446861/3778c44b-be45-4787-85bf-ce4dcbade8c7.png align="center")

When i hit it i can see the below dashboard.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677417432869/7c959e36-7989-462e-a98b-b4addb1040f0.png align="center")

Lets look at our AWS console where automatically our services were created.

S3 - our code will be stored in S3.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677417622523/e4f2f02f-e6c6-4f25-af7a-65cbc4882e70.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677417750494/2f5c694e-e75e-4d35-b8de-786b4932a768.png align="center")

API Gateway

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677418010709/332dcee7-f43e-4e50-876c-5759dafcdd63.png align="center")

Lambda

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677418078608/04f3f241-5b50-41c0-ac4a-f6759607fbd6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677418129863/056a6cc5-bfa2-41a8-ad41-ee521f42e7c0.png align="center")

Just by one click the process will be automated using a serverless application.

---

\--- Thank you for your time. Lets meet in the next blog for part -2 where I will be using database as well 😀--