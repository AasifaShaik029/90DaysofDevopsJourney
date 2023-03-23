---
title: "AWS Project Part 1 :CI/CD Pipeline Integration with AWS CodeCommit and CodeBuild."
datePublished: Thu Mar 23 2023 04:49:33 GMT+0000 (Coordinated Universal Time)
cuid: clfkmwimc00060al87xpk2ltu
slug: aws-project-part-1-cicd-pipeline-integration-with-aws-codecommit-and-codebuild
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679546939052/749546a0-ba41-42ff-aebd-b5d77044e6d2.png
tags: aws, projects, devops

---

Connecting IAM and Code Commit

### IAM

Inorder to use code commit, use need IAM user because you cant configure HTTP/HTTPS using root account. So let's create a User.

Create a User in IAM service.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678959283829/fbb95dd6-7e64-437b-a24e-6c779fbffe08.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678959361250/41fca155-43c4-4b89-b78a-32cf33977b67.png align="center")

Add code commit access permissions using add permissions tab and give Code Commit access to this IAM user by generating credentials and store it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679028974013/b8e6bc66-54f4-48ff-a41f-00af67ecca1b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679029005509/f14402fb-7c89-4da2-8561-bccf18673527.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679027987000/bc18de4d-b4b2-47a6-b09e-b2cc5f9eaf27.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679028004603/9cc733c1-5e53-40dd-8cb7-7a0d77d07c7e.png align="center")

Create a repository:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679028118597/9756eaa1-dbb0-42f9-a684-0420d2584939.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679028249275/5cada16c-91cb-4d1e-aa9b-ab7d3bcd9b39.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679028267572/c0dbe515-9ebd-4a07-a84c-95db20effa43.png align="center")

We will use this repo URL and clone it in our local system.

Open your any terminal and create was project repo where we will do our project and clone the repo which we copied above into the terminal

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679029198309/60ccad2d-a183-49b1-a230-e486557fad00.png align="center")

My repo is cloned now. I will go into my my-app folder and add index.html file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679029514135/5f906fd4-3298-4cb2-ad94-11b099f3bd2d.png align="center")

```plaintext
 git status
 git add .
 git commit -m "added index.html file"
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679029582122/a20fa548-5f27-4576-935c-e7da24aedc5f.png align="center")

So far I have created my index.html in my local server. Now I will push this file into AWS repository.

```plaintext
 git push origin master
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679029762110/20b96e4c-fb03-4c89-9cad-bae5ee58c916.png align="center")

Our file is successfully pushed into was repo

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679029954279/abe439fc-93d8-461a-ba41-cdf89833acef.png align="center")

Let's create a new branch and merge.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030409705/28c033e8-06b4-4484-a154-23b87a24144f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030472178/26969785-8289-4fe7-8d76-687ee5e77ec2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030516002/213bf249-b07e-41f7-84fc-728fdf9b47bc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030597670/fc426c43-f9c8-468e-9d9e-11c9ffa046ef.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030613938/e1fdcabc-5418-409b-9ebe-eeb19e29f0cb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030668356/ea5017de-754c-4a2d-8105-6c7bae417f42.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030712881/3217b9ac-ecee-4319-bd8c-62e7c4e268b7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679030759138/439b6cef-2a46-4ad7-bd4b-3d1c675424db.png align="center")

This is how we created a user who can access AWS code commit and we integrated our AWS Code commit with our local server and performed PUSH, MERGE operations.

---

### Code Build

### buildspec.yml file :

```plaintext
version: 0.2
phases:
  install:
    commands:
      - echo Installing NGINX
      - sudo apt-get update
      - sudo apt-get install nginx -y
  build:
    commands:
      - echo Build started on 'date'
      - cp index.html /var/www/html
  post_build:
    commands:
      - echo Configuring NGINX
artifacts:
  files:
    - '**/*'
```

CodeBuild compiles your source code, runs unit tests, and produces artifacts that are ready to deploy.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679032317719/552745c7-de12-41ce-86b2-933490d93577.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679032306647/2c154475-d808-4086-9ab0-76f5372eee0a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679034285169/cd7e273e-c54a-4588-9b4b-6fe3afee8a99.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679034449046/eaceece7-da25-4b6f-8213-caa9e9b6e5c1.png align="center")

Let's make use of S3 where our output artifacts will be stored inside it

Add s3 to artifacts

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679034657119/7f062de6-5be6-4de2-95d8-6ad95f961a9e.png align="center")

But before that create a folder as object in the created bucket

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679034814956/fd2bcbec-7d5c-415e-b9f1-5706590bf4dc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679034848342/5475c7ad-4bf8-4305-b9a2-2c6c176c466b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679034971805/0080e38b-1dfb-490e-98f5-585d919c326f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679485276099/4b0b392d-ee92-4e94-9ef8-2179fb9cb943.png align="center")

Our build is succeeded and artifacts were stores in s3

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679486806355/bce0c255-8df7-428a-a4b3-431304c94492.png align="center")