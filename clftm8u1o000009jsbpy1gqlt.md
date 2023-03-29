---
title: "AWS Serverless Project - Part 2"
datePublished: Wed Mar 29 2023 11:41:03 GMT+0000 (Coordinated Universal Time)
cuid: clftm8u1o000009jsbpy1gqlt
slug: aws-serverless-project-part-2
tags: devops, serverless

---

Lets create one more project in our serverless-projects directory.

Clone the repo which contains 2 branches , we are having our code in our dev branch so lets move to that branch

[https://github.com/AasifaShaik029/aws-node-http-api-project.git](https://github.com/AasifaShaik029/aws-node-http-api-project.git)

add serverless user name to connect using

```plaintext
serverless --org=aasifa
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677562542451/4abf6f03-85d1-4401-81f0-3c6724fd5d3c.png align="center")

We need necessary modules to deploy the app

```plaintext
npm install
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677562730005/2801aada-a3ce-4095-8484-67fed536b7b0.png align="center")

you can see node modules added. Now deploy the application

```plaintext
sls deploy
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677562792545/628b3ade-3450-44c0-af35-d3ebf9d4137c.png align="center")

We will get the endpoint URLs for get (for the response) post ( to insert the data) and put ( to update the data)

We will be using POSTMAN tool . It is a simple Graphic User Interface for sending and viewing HTTP requests and responses.

add all the request and click send for the response. The data will be updated.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677563138550/7ebd9d8b-7d33-424c-84de-e8aa3f203366.png align="center")